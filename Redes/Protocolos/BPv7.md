---
disciplina: Redes
tags:
  - redes
  - dtn
  - espaco
---

# BPv7 — Bundle Protocol version 7

## O que é e que problema resolve
O **Bundle Protocol v7** (RFC 9171) é o protocolo de **rede/aplicação** da arquitetura **DTN** (Delay-Tolerant Networking). A Internet assume um caminho fim-a-fim contínuo e RTT baixo; quando isso não existe (espaço, zonas rurais, catástrofes, redes táticas), o BP cria uma **overlay store-and-forward**: os dados são guardados em *bundles* e avançam salto a salto sempre que há contacto.

Chave conceptual: **não há sessão fim-a-fim**. Cada nó é um *bundle node* que recebe, armazena e reencaminha.

## Modelo
```
Aplicação
  BPv7  ← nós com armazenamento (custódia opcional)
  Convergence Layer (LTP / TCPCL / UDPCL / QUIC)
  Rede física
```

- **Bundle** — unidade de dados: bloco primário + blocos canónicos + payload.
- **Endpoint ID (EID)** — endereço no formato URI, tipicamente `dtn://no/…` ou `ipn:1.2`.
- **Bundle node** — qualquer nó que processa bundles (source, destination, forwarder).

## Estrutura do bundle
- **Primary block**: versão, flags, *creation timestamp* (data + sequência), *lifetime*, CRC, EIDs de origem/destino/report.
- **Canonical blocks**: extensões (ex.: *bundle age*, *hop count*, *previous node*, BPSec).
- **Payload block**: dados da aplicação.

Cada bloco tem o seu próprio CRC e flags (*must be replicated*, *discard if unrecognized*).

## Funcionamento
1. A aplicação entrega um bundle ao nó de origem.
2. O nó guarda-o e tenta enviar pela *convergence layer* disponível.
3. O nó seguinte valida, armazena e reencaminha (ou entrega, se for o destino).
4. O bundle expira ao fim do **lifetime**; pode ser descartado antes por falta de recursos ou política.

### Custódia
O v7 **removeu os *custody signals*** do v6 — a “custódia” passou a ser uma decisão local/política (o nó pode aceitar responsabilidade e retransmitir), mas não há um protocolo de transferência de custódia normalizado como no v6. Isto é uma diferença importante para exame.

### Fragmentação
Um bundle pode ser dividido em **fragmentos** (com *offset* e comprimento no payload) ao atravessar um salto com MTU/contacto limitado. O destino reassembla.

### Status reports
O destino (ou nós intermédios) pode gerar **status reports** (receção, entrega, reencaminhamento, eliminação) enviados ao EID de report. Permitem saber o que aconteceu sem ACK fim-a-fim.

## Convergence layers
O BP precisa de uma camada que o transporte por baixo:
- **TCPCLv4** (RFC 9174) — sobre TCP, para Internet.
- **LTPCL** — sobre LTP, para espaço profundo.
- **UDPCL** — sobre UDP datagramas.
- **QUICCL** — emergente, para multiplexagem e 0-RTT.

## Segurança — BPSec
O **BPSec** (RFC 9172) define blocos de segurança aplicáveis a bundles (ou a blocos):
- **BCB** (Block Integrity Block) — integridade/autenticação;
- **BCC** (Block Confidentiality Block) — confidencialidade.
Os algoritmos são negociados por contexto de segurança. Isto permite proteger *partes* do bundle, não só o todo.

## Comparação
| | BPv7 | TCP/IP | CFDP |
|---|---|---|---|
| Caminho | store-and-forward | fim-a-fim | ficheiro fim-a-fim |
| Disrupção | tolera nativamente | falha | tolera com relay |
| Unidade | bundle | byte | ficheiro |
| Endereço | EID | IP:porta | entity+seq |

## Casos de uso
- **Internet Interplanetária** (IPN) — o caso de referência.
- Redes rurais/offline (ex.: *Delay-Tolerant Twitter*).
- Comunicações de emergência e militares.
- Integração de redes heterogéneas com contactos intermitentes.

## Pontos-chave para exame
1. BPv7 é **store-and-forward** e **não fim-a-fim**.
2. Endereçamento por **EID** (`dtn:` / `ipn:`).
3. **Lifetime** controla a validade; **fragmentação** por salto.
4. v7 **removeu os custody signals** do v6.
5. Segurança via **BPSec** (BIB/BCB).
6. Precisa de **convergence layers** (LTP, TCPCL, UDPCL, QUIC).

## Referências
- RFC 9171 (BPv7), RFC 9172 (BPSec), RFC 9174 (TCPCLv4), RFC 4838 (arquitetura DTN).
