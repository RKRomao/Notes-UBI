---
disciplina: Redes
tags:
  - redes
  - quic
  - transporte
---
# QUIC

Protocolo de transporte **seguro e multiplexado** desenvolvido pela IETF, que serve de base ao **HTTP/3**.

## Porque existe

O QUIC nasce como *workaround* para o **head-of-line blocking do TCP** (ver [[HTTP]]): no TCP, a perda de um único segmento bloqueia todos os fluxos que partilham a ligação. O QUIC resolve isso colocando a multiplexagem **no próprio transporte**, e não na aplicação.

## Características

- **Corre sobre UDP** — o UDP tem menos RTT (*round-trips*) e não impõe a semântica de ordem do TCP. É, na prática, «um protocolo de transporte sobre um protocolo de transporte».
- **TLS 1.3 integrado** — a negociação criptográfica faz parte do handshake do QUIC, em vez de ser uma camada separada por cima do TCP.
- **Fluxos independentes** — a ordem é garantida **por fluxo**, não por ligação. Uma perda num fluxo não bloqueia os restantes.
- **Connection ID** — a ligação é identificada por um **ID de conexão** e não pelo par de IPs. Por isso sobrevive à mudança de rede (ex.: Wi-Fi → 4G), algo que o TCP não permite.
- **Handshake em 1 RTT**, com retomas em **0 RTT**.

## 0-RTT

- Na **primeira ligação** a um servidor há **1 RTT** de handshake.
- Nas **ligações seguintes** o cliente pode retomar uma ligação anterior e enviar dados imediatamente: **0 RTT**.
- O envio antecipado (*early data*) depende de:
  - o servidor **aceitar** os dados antecipados;
  - haver **proteção contra repetição** (*replay protection*), já que os dados 0-RTT podem ser reutilizados por um atacante.

> [!warning] Aviso para o teste
> **0 RTT ≠ 0 latência.** 0 RTT significa apenas **menos latência** (não é preciso esperar por um round-trip completo antes de enviar dados).

## QUIC vs TCP

| | TCP | QUIC |
| --- | --- | --- |
| Ordem | Ordem comum a toda a ligação | Ordem **por fluxo** |
| Identidade da ligação | Par de IPs + portas | **Connection ID** |
| Criptografia | Externa (TLS por cima) | **Integrada** (TLS 1.3) |
| Perda de pacote | Bloqueia todos os fluxos (HOL) | Bloqueia apenas o fluxo afetado |
| Mudança de rede | Ligação cai | Ligação mantém-se |

## Ligações

- [[HTTP]] — HTTP/3 corre sobre QUIC
- [[Camada de Transporte]]
- [[Pilha TCP-IP]]
