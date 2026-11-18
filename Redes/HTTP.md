---
disciplina: Redes
tags:
  - redes
  - http
  - aplicação
---
# HTTP

Protocolo de **aplicação** baseado no modelo **pedido → resposta** entre cliente e servidor. Cada versão foi criada para resolver um problema de desempenho da anterior.

## Evolução

### HTTP/1.0
- Uma ligação TCP por cada pedido.
- Uma única **fila**: os pedidos são servidos sequencialmente.
- **Head-of-line blocking (HOL)**: para fechar uma chamada é preciso terminar todos os pedidos anteriores; um pedido lento bloqueia os seguintes.

### HTTP/1.1
- Introduziu ligações persistentes (*keep-alive*) e *pipelining* — várias filas, mas na mesma ligação.
- Problemas:
  - Cada ligação tem custos (handshake TCP, estado no servidor).
  - A perda de uma ligação obriga a reabrir tudo.
  - O **HOL blocking mantém-se**: as respostas têm de ser entregues pela ordem dos pedidos.
- **Solução prática dos browsers:** abrir várias ligações TCP em paralelo (tipicamente 6 por domínio), o que multiplica o custo.

### HTTP/2
- **Multiplexagem de fluxos** sobre **uma única ligação TCP**: vários pedidos e respostas em paralelo, com identificação por *stream*.
- Resolve o HOL blocking **ao nível do HTTP**, mas **transfere o problema para o TCP**: se um segmento TCP se perde, todos os fluxos ficam bloqueados à espera da retransmissão.
- Também introduziu compressão de cabeçalhos (HPACK) e *server push*.

### HTTP/3
- Passa a correr sobre **[[QUIC]]** (e não sobre TCP), eliminando o HOL blocking entre fluxos.
- Cada fluxo é independente: a perda de um pacote só afeta o fluxo a que pertence.

## Linha temporal do problema

```
HTTP/1.0  → fila única, HOL ao nível do pedido
HTTP/1.1  → várias ligações/filas, custo elevado, HOL mantém-se
HTTP/2    → multiplexagem, mas HOL passa para o TCP
HTTP/3    → QUIC sobre UDP, fluxos independentes, sem HOL entre fluxos
```

## Handshake e latência

Com HTTP/2 sobre TCP + TLS, antes do primeiro byte de dados são necessários:

- **1 RTT** para o handshake TCP;
- **2 RTT** para o handshake TLS 1.2 (ou **1 RTT** com TLS 1.3).

Ou seja, cerca de **3 RTT** no caso clássico (TCP + TLS 1.2). Com [[QUIC]]+TLS 1.3 o handshake é combinado e cabe em **1 RTT** (ou **0 RTT** em retomas).

## Ligações

- [[QUIC]]
- [[Camada de Transporte]]
- [[Pilha TCP-IP]]
