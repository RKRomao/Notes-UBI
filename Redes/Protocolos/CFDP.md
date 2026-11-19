---
disciplina: Redes
tags:
  - redes
  - espaco
  - dtn
---

# CFDP — CCSDS File Delivery Protocol

## O que é e que problema resolve
O **CFDP** (CCSDS 727.0-B-5) é um protocolo de **transferência de ficheiros** concebido para ligações espaciais: RTT de segundos a dezenas de minutos, ligações que caem e voltam, largura de banda assimétrica e memória limitada a bordo. O TCP não serve nestes cenários porque:

- o *handshake* e o *slow start* desperdiçam a janela de contacto;
- perdas são interpretadas como congestão;
- a ligação pode ser unidirecional durante longos períodos.

O CFDP resolve isto com **transferência orientada a ficheiro** (e não a fluxo), confirmações diferidas e retransmissão seletiva, podendo operar *store-and-forward* por nós intermédios.

## Posição na pilha
Fica **acima** do enlace (TM/TC, AOS, Proximity-1) e é independente do transporte: pode correr diretamente sobre o enlace, sobre UDP/IP, ou sobre LTP quando se quer fiabilidade em DTN.

```
Aplicação (ficheiro)
      CFDP
   LTP | UDP/IP | Enlace CCSDS
      Física
```

## Modelo de operação
Uma **transação CFDP** é identificada por:

- **Entity ID** do emissor e do recetor (identificadores CCSDS);
- **Transaction Sequence Number** (incrementado por transação).

O emissor abre a transação com metadados, envia os dados em **File Data PDUs**, sinaliza o fim com **EOF**, e o recetor confirma ou pede retransmissões. O estado da transação é mantido em ambos os lados até conclusão (ou *fault*).

## PDUs
| PDU | Função |
|---|---|
| **Metadata** | nome, tamanho, checksum, opções (CRC, segmentação) |
| **File Data** | payload, offset, comprimento; flag de fim de ficheiro |
| **EOF** | fim dos dados, checksum final, tamanho |
| **Finished** | confirmação final do recetor |
| **ACK** | confirmação de diretivas (EOF, Finished) |
| **NAK** | pedido de retransmissão com lista de segmentos em falta |
| **Keep Alive** | evita *timeouts* em ligações longas sem tráfego |

## Classes de serviço
- **Classe 1 — Unacknowledged**: envio cego, sem confirmação nem retransmissão. Usada quando a fiabilidade é garantida por baixo (ex.: LTP) ou quando não há canal de retorno.
- **Classe 2 — Acknowledged**: fiável. Duas variantes:
  - **Deferred NAK**: o recetor espera até ao fim (ou até um temporizador) e pede tudo o que falta de uma vez — eficiente em ligações de RTT enorme.
  - **Immediate NAK**: pede assim que deteta lacuna — útil em ligações com RTT baixo e taxa de erro alta.

> Nota: o CFDP standard define apenas classes 1 e 2; “classes 3/4” que aparecem em algumas fontes são extensões/implementações, não o standard CCSDS.

## Temporizadores
O comportamento é dominado por timers, todos configuráveis por transação:

- **ACK timer** — quando esperar pelo ACK antes de retransmitir a diretiva;
- **NAK timer** — quando o recetor deve emitir NAK;
- **Inactivity timer** — declara *fault* se nada acontece;
- **EOF timer / Finished timer** — protegem o fecho da transação.

## Segmentação e retransmissão
O ficheiro é dividido em **segmentos** com offset explícito. O recetor mantém um mapa de buracos e pede apenas os segmentos em falta (NAK seletivo), evitando reenviar o ficheiro todo. Isto é essencial quando um contacto de 10 minutos custa horas a chegar.

## Store-and-forward
Um nó intermédio pode receber o ficheiro completo e reencaminhá-lo mais tarde (“waypoint”). Isto permite construir uma cadeia Terra → orbiter → lander sem ligação fim-a-fim simultânea.

## Segurança
O CFDP não traz cifra própria; a proteção é delegada ao enlace (SDLS/CCSDS Space Data Link Security) ou ao BPv7/BPSec quando usado em DTN. O checksum do ficheiro protege integridade, não autenticidade.

## Casos de uso
- Download de dados científicos de sondas (Mars Express, rovers).
- Upload de software e comandos.
- Relay entre orbiter e lander.

## Comparação rápida
| | CFDP | FTP/SFTP | BPv7 |
|---|---|---|---|
| Unidade | ficheiro | ficheiro | bundle |
| Fiabilidade | classe 2, NAK seletivo | TCP | custódia/CL |
| Tolerância a disrupção | alta | baixa | muito alta |
| Latência alvo | minutos-horas | ms | qualquer |

## Pontos-chave para exame
1. CFDP é **orientado a ficheiro**, não a fluxo.
2. Existem **duas classes**: 1 (sem ACK) e 2 (com ACK; deferred vs immediate NAK).
3. A eficiência vem do **NAK seletivo** e dos **timers longos**.
4. Suporta **store-and-forward** por nós intermédios.
5. Não fornece segurança própria — usa SDLS/BPSec.

## Referências
- CCSDS 727.0-B-5, *CFDP*.
- CCSDS 732.0-B (AOS), 235.1-B (SDLS).
