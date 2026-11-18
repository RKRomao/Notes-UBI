---
disciplina: Redes
tags:
  - redes
  - transporte
---

# Camada de Transporte

Responsável pela comunicação **fim-a-fim entre processos** (não apenas entre máquinas, como o IP). É aqui que se resolve a **fiabilidade**: entrega ordenada, sem perdas nem duplicados.

## Mecanismos de fiabilidade

| Mecanismo | Para que serve |
| --- | --- |
| **ACK** (*acknowledgement*) | Confirmação positiva de receção |
| **NACK** | Confirmação negativa — pede reenvio de um segmento em falta |
| **Checksum** | Deteta corrupção de bits no segmento |
| **Timeout** | Temporizador que dispara a retransmissão se o ACK não chegar |
| **Números de sequência** | Permitem reordenar e detetar duplicados |
| **Janela (window)** | Controlo de fluxo: quantos dados podem estar em trânsito |

## TCP

- Estabelece uma **ligação** (*3-way handshake*: SYN → SYN-ACK → ACK).
- Garante **uma ordem comum** para todos os dados da ligação.
- A ligação é identificada pelo par de **endereços IP + portas**; se o IP mudar, a ligação deixa de existir.
- Garante fiabilidade, mas sofre de **head-of-line blocking**: se um segmento se perde, todos os dados seguintes ficam bloqueados à espera da retransmissão — mesmo que já tenham chegado.

## UDP

- **Datagramas**, sem ligação, sem garantia de entrega, sem ordem e sem controlo de congestionamento.
- Muito leve: poucos *round-trips* e baixa latência.
- É a base do [[QUIC]], que lhe acrescenta fiabilidade e segurança.

## Portas

O IP entrega o pacote à máquina; a **porta** entrega-o ao processo certo (ex.: 80/443 para HTTP/HTTPS, 53 para DNS).

> [!tip] Regra prática
> **TCP** = fiabilidade e ordem, ao custo de latência. **UDP** = velocidade, ao custo de garantias. É por isso que o [[QUIC]] constrói a sua fiabilidade sobre UDP.

## Ligações

- [[Pilha TCP-IP]]
- [[HTTP]] — onde o *head-of-line blocking* do TCP se torna visível
- [[SCTP]] — alternativa ao TCP com *multi-streaming* e *multi-homing*
