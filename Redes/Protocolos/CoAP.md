---
disciplina: Redes
tags:
  - redes
  - iot
  - aplicacao
---

# CoAP — Constrained Application Protocol

## O que é e que problema resolve
O **CoAP** (RFC 7252) é um protocolo de aplicação **REST sobre UDP**, desenhado para dispositivos com pouca RAM/CPU/energia e redes com perdas (6LoWPAN, TSCH). O HTTP é demasiado pesado: cabeçalhos em texto, TCP, handshakes longos. O CoAP mantém o modelo **GET/POST/PUT/DELETE + URIs + códigos de resposta**, mas com um cabeçalho fixo de **4 bytes** e transporte datagrama.

É o “HTTP do IoT”.

## Modelo
- **Recursos** identificados por URIs (`coap://host:5683/sensors/temp`).
- **Métodos**: GET, POST, PUT, DELETE (e FETCH/PATCH em extensões).
- **Códigos de resposta** no estilo HTTP (`2.05 Content`, `4.04 Not Found`).
- **Content-format** (equivalente a *Content-Type*) — ex.: `application/senml+json`.
- **Opções** codificadas de forma compacta (delta encoding).

## Mensagens e tipos
Cada mensagem tem:

- **Message ID** (16 bits) — para deduplicação e emparelhamento de ACK;
- **Token** (0–8 bytes) — correlaciona pedido/resposta (permite várias em voo);
- **Code** — método ou resposta;
- **Options + payload**.

Tipos:

| Tipo | Significado |
|---|---|
| **CON** (Confirmable) | requer ACK; fiabilidade fim-a-fim |
| **NON** (Non-confirmable) | sem ACK; melhor esforço |
| **ACK** | confirma uma CON |
| **RST** | reset / mensagem inválida |

Fiabilidade é **opcional e fim-a-fim**: CON + retransmissão com *timeout* exponencial. Se a resposta não cabe no ACK, vem numa mensagem separada (*piggybacked* vs *separate response*).

## Block-wise transfer
O **Block1/Block2** (RFC 7959) permite transferir payloads maiores que o MTU em blocos, com negociação de tamanho (16–1024 bytes) e *offset*. Essencial em 6LoWPAN (MTU ~127 bytes).

## Observe (pub/sub leve)
O **Observe** (RFC 7641) permite ao cliente **registar-se** num recurso e receber **notificações** quando muda, sem *polling*. É a alternativa leve ao MQTT para dados periódicos: uma GET com opção Observe → respostas NON periódicas com número de sequência.

## Descoberta
- `/.well-known/core` lista recursos (formato CoRE Link Format, RFC 6690).
- *Resource discovery* por multicast (`ff02::fd`) — permite encontrar sensores sem configuração.

## Congestion control
Como corre sobre UDP, o CoAP implementa um controlo simples:
- **NSTART**: número máximo de interações CON simultâneas com um peer (tipicamente 1);
- **PROBING_RATE**: taxa máxima de envio quando não há resposta;
- *backoff* exponencial em retransmissões.

Não é um controlo de congestão completo como TCP — é adequado a redes de baixo tráfego.

## Segurança
- **DTLS** (RFC 6347) sobre UDP — modo *Pre-Shared Key*, certificados ou RPK.
- **OSCORE** (RFC 8613) — proteção **fim-a-fim** ao nível da aplicação, mesmo através de *proxies*; usa COSE.
- **DTLS 1.3** e **TLS 1.3** para gateways.
- *Proxies* CoAP→HTTP permitem integração com a Web.

## CoAP vs HTTP vs MQTT
| | CoAP | HTTP/1.1 | MQTT |
|---|---|---|---|
| Transporte | UDP | TCP | TCP |
| Modelo | request/response (+Observe) | request/response | pub/sub |
| Header | 4 bytes binário | texto | 2 bytes fixo |
| Ideal para | dispositivos restritos | Web | telemetria many-to-many |
| Segurança | DTLS/OSCORE | TLS | TLS |

## Casos de uso
- Sensores e atuadores em 6LoWPAN/TSCH.
- Smart home (base do Thread → usa CoAP/6LoWPAN).
- Monitorização industrial e cidades.

## Pontos-chave para exame
1. CoAP = **REST sobre UDP**, cabeçalho de 4 bytes.
2. **CON/NON/ACK/RST** + Message ID e Token.
3. **Block-wise** resolve o MTU pequeno.
4. **Observe** dá notificações sem polling.
5. Segurança por **DTLS** ou **OSCORE** (fim-a-fim).

## Referências
- RFC 7252 (CoAP), RFC 7959 (Block-wise), RFC 7641 (Observe), RFC 8613 (OSCORE), RFC 6690 (CoRE Link Format).
