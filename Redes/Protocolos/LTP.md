---
disciplina: Redes
tags:
  - redes
  - espaco
  - dtn
  - transporte
---

# LTP — Licklider Transmission Protocol

## O que é e que problema resolve
O **LTP** (RFC 5326) é um protocolo de transporte para **redes de atraso e disrupção (DTN)**, pensado para ligações em que o RTT pode ser de minutos a horas e em que o canal pode estar indisponível durante longos períodos. O nome homenageia J.C.R. Licklider.

Não tenta ser um TCP “melhorado”: assume que **não há caminho fim-a-fim estável** e que a fiabilidade deve ser negociada **salto a salto**, com estado retido no recetor.

## Onde encaixa
```
BPv7 (bundle)
   LTP          ← fiabilidade salto-a-salto
   UDP (porta 1113)
   Enlace / Proximity-1
```
O LTP é a *convergence layer* típica do BP em ligações de espaço profundo. Também pode correr diretamente sobre o enlace, sem IP.

## Estrutura: sessões, blocos e segmentos
- **Sessão** — associação unidirecional entre dois endpoints LTP (engine IDs). Uma sessão transporta vários **blocos**.
- **Bloco** — unidade de dados entregue à camada superior (ex.: um bundle). Pode ser:
  - **Red part** (fiável): retransmitida até confirmação;
  - **Green part** (não fiável): enviada uma vez, sem retransmissão.
  Esta separação permite pôr os metadados/cabeçalho no vermelho e payload opcional no verde.
- **Segmento** — fragmento de bloco, com offset e comprimento.

## Mecanismo de fiabilidade: checkpoints e reports
O LTP não usa ACKs positivos por pacote. Usa:

- **CP (Checkpoint)** — “já enviei até este ponto; pede relatório”.
- **RS (Report Segment)** — relatório do recetor com os intervalos **recebidos** (claim) e o *upper bound* do que falta.
- **RA (Report Acknowledgement)** — confirma o RS.
- **CAS (Checkpoint Ack / Cancel)** e **CS** para fecho de bloco.

O emissor, ao receber um RS, calcula os buracos e **retransmite apenas esses segmentos** (*selective repeat*). Trabalha com **timers** (checkpoint timer, report timer, retransmission timer, cancel timer) ajustados ao RTT enorme.

## “Blocking” e entrega
Um bloco só é entregue à camada superior quando **todo o red part** está completo. Isto evita entregar bundles truncados. O green part pode chegar fora de ordem e não bloqueia.

## Assimetria e canais unidirecionais
Desenhado para ligações em que pode não haver retorno imediato: o emissor pode enviar vários checkpoints antes de receber reports; o recetor agrega e responde quando puder. Há um modo **unreliable** (só green) para quando não há retorno nenhum.

## Cancelamento e gestão de estado
- **Cancel segment** do emissor pede ao recetor que liberte estado.
- **Cancel Ack** confirma.
- Limites de sessão/bloco evitam crescimento infinito de estado.

## Segurança
O LTP não cifra. Assume-se que a proteção vem de BPSec (no bundle) ou de SDLS (no enlace). Há validação de comprimento e offsets para evitar corrupção, mas não autenticação.

## Parâmetros típicos
- Tamanho de segmento ~1400 bytes (cabe em UDP sem fragmentação IP).
- Nº máximo de checkpoints por bloco, tamanho de sessão, timers por classe de serviço.

## Comparação
| | LTP | TCP | CFDP classe 2 |
|---|---|---|---|
| Âmbito | salto-a-salto DTN | fim-a-fim | ficheiro fim-a-fim |
| Feedback | reports negativos | ACKs | NAK/ACK |
| RTT suportado | minutos-horas | ms | minutos |
| Unidade | bloco | byte stream | ficheiro |

## Casos de uso
- Ligações Terra–Marte (Mars Reconnaissance Orbiter, rovers).
- Cenários militares/táticos com interrupções.
- Qualquer ligação em que o transporte terrestre falha por RTT ou disrupção.

## Pontos-chave para exame
1. LTP é **salto-a-salto**, não fim-a-fim.
2. **Red part** fiável vs **green part** não fiável.
3. Fiabilidade por **checkpoint + report + retransmissão seletiva**.
4. Corre tipicamente sobre **UDP (1113)**.
5. É a *convergence layer* clássica do **BPv7**.

## Referências
- RFC 5326 (LTP), RFC 5325 (motivação), RFC 7122 (datagram convergence layer).
