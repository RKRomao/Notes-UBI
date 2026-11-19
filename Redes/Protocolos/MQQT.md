---
disciplina: Redes
tags:
  - redes
  - iot
  - aplicacao
---

# MQTT — Message Queuing Telemetry Transport

## O que é e que problema resolve
O **MQTT** é um protocolo de mensagens **publicador/subscritor** extremamente leve, criado na IBM (1999) para telemetria sobre ligações caras e instáveis. Standardizado pela OASIS: **3.1.1** (2014) e **5.0** (2019). É hoje o protocolo dominante em IoT e IIoT.

Resolve o problema de **many-to-many** com dispositivos que dormem, perdem rede e têm pouca energia: em vez de cada dispositivo falar com cada consumidor, todos falam com um **broker** central que faz o *fan-out*.

## Arquitetura
```
Publisher ──┐
            ├──► Broker ──► Subscriber
Publisher ──┘        │
                     └──► Subscriber
```

- **Broker** — servidor que recebe, filtra e distribui mensagens (Mosquitto, HiveMQ, EMQX).
- **Client** — qualquer publicador ou subscritor.
- **Topic** — string hierárquica separada por `/` (ex.: `casa/andar1/temp`).
- **Subscription** — filtro de tópicos com *wildcards*:
  - `+` — um nível (`casa/+/temp`);
  - `#` — todos os níveis abaixo (`casa/#`).

Não há endereçamento direto entre clientes: **desacoplamento** total (espaço, tempo e sincronização).

## Modelo de mensagens
- **PUBLISH** — mensagem para um tópico.
- **Payload** — opaco (binário/JSON/SenML).
- **QoS**:
  - **0 — at most once**: “fire and forget”; pode perder-se.
  - **1 — at least once**: PUBLISH → PUBACK; pode duplicar.
  - **2 — exactly once**: PUBLISH → PUBREC → PUBREL → PUBCOMP; sem duplicados, maior custo.

## Pacotes de controlo
| Pacote | Função |
|---|---|
| CONNECT / CONNACK | sessão e capacidades |
| PUBLISH | dados |
| PUBACK / PUBREC / PUBREL / PUBCOMP | QoS 1/2 |
| SUBSCRIBE / SUBACK | subscrever |
| UNSUBSCRIBE / UNSUBACK | remover |
| PINGREQ / PINGRESP | keepalive |
| DISCONNECT | fecho |
| AUTH (v5) | autenticação estendida |

Formato: **fixed header (2 bytes)** + *variable header* + *payload*. É isto que dá o baixo overhead.

## Funcionalidades importantes
- **Retained message** — o broker guarda a última mensagem de um tópico e entrega-a a novos subscritores (estado atual).
- **Last Will and Testament (LWT)** — mensagem publicada pelo broker se o cliente desaparecer (deteção de falha).
- **Keepalive** — PINGREQ periódico; ausência → LWT.
- **Clean session / persistent session** — manter subscrições e mensagens em fila para clientes offline.
- **QoS + offline queue** — entrega diferida a dispositivos intermitentes.

## MQTT 5.0
- **Reason codes** detalhados.
- **Properties** (user properties, message expiry, content type, response topic, correlation data).
- **Shared subscriptions** (`$share/grupo/topico`) — balanceamento entre consumidores.
- **Session expiry**, **topic aliases**, **request/response**.
- **AUTH** para autenticação reforçada (SASL-like).

## Segurança
- **TLS** (porta 8883) — confidencialidade e integridade.
- **Autenticação**: username/password, certificados de cliente (mTLS), JWT em v5.
- **ACL** por tópico no broker.
- Riscos: broker exposto, tópicos sem ACL, *wildcards* que expõem dados, ausência de cifra em 1883.

## Escalabilidade
- Brokers *clusterizados* (partilha de subscrições).
- **Bridges** entre brokers (hierarquia/edge-cloud).
- Milhões de conexões por nó com *event loops* eficientes.

## Comparação
| | MQTT | CoAP | AMQP | HTTP |
|---|---|---|---|---|
| Modelo | pub/sub | req/resp | filas/rotas | req/resp |
| Transporte | TCP (ou WS/QUIC) | UDP | TCP | TCP |
| Overhead | muito baixo | muito baixo | médio | alto |
| Ideal | telemetria | restrito/6LoWPAN | enterprise | Web |

## Casos de uso
- Telemetria industrial (IIoT), smart home, frota/logística.
- Agricultura, energia, cidades.
- Integração edge → cloud.

## Pontos-chave para exame
1. MQTT = **pub/sub via broker**, desacoplamento total.
2. **QoS 0/1/2** e o custo crescente de cada um.
3. **Retained**, **LWT**, **keepalive**, **sessão persistente**.
4. Tópicos hierárquicos com `+` e `#`.
5. Segurança por **TLS + mTLS + ACL**.
6. v5 acrescenta properties, reason codes, shared subscriptions.

## Referências
- OASIS MQTT 3.1.1 e MQTT 5.0.
