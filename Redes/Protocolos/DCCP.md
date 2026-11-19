---
disciplina: Redes
tags:
  - redes
  - transporte
---

# DCCP — Datagram Congestion Control Protocol

## O que é e que problema resolve
O **DCCP** (RFC 4340) é um protocolo de **transporte orientado a conexão mas não fiável**: entrega datagramas como o UDP, mas com **controlo de congestão** como o TCP. Preenche o vazio entre:

- **UDP**: leve, mas sem controlo de congestão → risco de colapso e injustiça com TCP;
- **TCP**: fiável e ordenado, mas retransmissões e *head-of-line blocking* matam aplicações de tempo real.

Aplicações-alvo: *streaming*, VoIP, jogos — querem **timing** mais do que **fiabilidade**.

## Características centrais
- **Orientado a conexão**: *handshake* explícito, estado mantido nas duas pontas, opções negociadas.
- **Não fiável**: sem retransmissão de dados; sem entrega ordenada.
- **Controlo de congestão plugável**: o **CCID** (Congestion Control ID) escolhe o algoritmo.
- **Entrega de dados com seq/ack**: o recetor sabe o que perdeu, mas não pede reenvio (a aplicação decide).
- **Multiplexagem de serviços** por porta, como TCP/UDP.

## Handshake e negociação
O DCCP faz um *three-way handshake* com **negociação de features**:

- pacotes **DCCP-Request / DCCP-Response / DCCP-Ack**;
- features negociáveis: CCID, *short sequence numbers*, *checksum coverage*, *service code*, etc.

Isto é mais rico que o handshake TCP: as duas pontas acordam semântica antes de trocar dados.

## Tipos de pacote
| Pacote | Função |
|---|---|
| Request / Response / Ack | abertura de conexão |
| Data | dados |
| Ack | confirmação (com *ack vector*) |
| DataAck | dados + ack no mesmo pacote |
| CloseReq / Close | fecho |
| Reset | aborto |
| Sync / SyncAck | ressincronização após perda de estado |

## Numeração e ACKs
- Números de sequência de **48 bits**.
- **ACK vectors** descrevem intervalos recebidos, permitindo ao emissor estimar perdas sem retransmitir.
- Opções: *Elapsed Time*, *Timestamp*, *Timestamp Echo*, *CCID-specific*.

## Controlo de congestão (CCIDs)
- **CCID 2** — *TCP-like*: AIMD com janela, reage como TCP quando há perdas. Bom para fluxos que querem justiça com TCP.
- **CCID 3** — **TFRC** (*TCP-Friendly Rate Control*): ajusta a taxa suavemente com base em perda/RTT; melhor para média porque evita oscilações bruscas.
- **CCID 4** — TFRC para pacotes pequenos (VoIP).

O ponto pedagógico: o DCCP **separa fiabilidade de controlo de congestão** — pode ter a segunda sem a primeira.

## Segurança
- Sem cifra nativa; usa-se IPsec ou DTLS por cima.
- *Service codes* dificultam scanners casuais, não são segurança.
- Validação de sequência e *checksum* (cobertura configurável) contra corrupção.

## Implementação e adoção
- Disponível no **kernel Linux** (`dccp` module), com suporte limitado em utilizadores.
- Concorreu com **SCTP** e, mais tarde, **QUIC** (que resolve o mesmo problema com fiabilidade por fluxo). Por isso a adoção real é baixa, apesar de ser standard.

## Comparação
| | UDP | DCCP | TCP | SCTP |
|---|---|---|---|---|
| Fiável | não | não | sim | sim (por stream) |
| Ordenado | não | não | sim | por stream |
| Congestão | não | sim (plugável) | sim | sim |
| Handshake | não | sim | sim | sim (4-way) |
| HOL blocking | n/a | não | sim | não (entre streams) |

## Casos de uso
- Streaming de vídeo/áudio com controlo de taxa.
- VoIP (CCID 4).
- Jogos e telemetria em tempo real.

## Pontos-chave para exame
1. DCCP = **datagramas + controlo de congestão**, sem fiabilidade.
2. **CCID 2** ≈ TCP-like; **CCID 3/4** = TFRC.
3. Tem **handshake e negociação de features** (mais rico que TCP).
4. Separa **fiabilidade** de **congestão** — é a ideia central.
5. Perdeu terreno para **QUIC**.

## Referências
- RFC 4340 (DCCP), RFC 4341 (CCID 2), RFC 4342 (CCID 3), RFC 4828 (CCID 4).
