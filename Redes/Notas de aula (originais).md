---
disciplina: Redes
tags:
  - redes
  - arquivo
---
# Notas de aula (originais)

> [!info] Arquivo
> Registo em bruto das aulas, mantido para não perder nenhuma referência. O conteúdo organizado e desenvolvido está em [[Arquiteturas Avançadas Redes (AAR)]].

## Matéria falada

- Pilha protocolar TCP/IP 
- Avisos para teste
- UDP e mecanismos de comunicação
- HTTP/3 e QUIC
- Camada responsável pela fiabilidade
- MPLS (Multiprotocol Label Switching)

## Apontamentos

- ACK / NACK
- Checksum
- Timeout

### HTTP

- **HTTP/1.0** — era uma fila só; *head-of-line blocking* (para fechar chamadas tem de acabar os processos todos).
- **HTTP/1.1** — acrescentou as filas, mas com mais custo (cada ligação tem custos); perda de ligação: a entrega pode esperar; HOL mantém-se.
- **HTTP/2** — permite vários fluxos (vários fluxos podem esperar); o HOL passou para a camada TCP.
- Resolveram e gerou **TCP head-of-line blocking**.
- A solução vem no **HTTP/3** e no **QUIC** (sempre que há perda de pacote não ficamos à espera).
- QUIC usa UDP porque o UDP tem menos RTT (usa apenas 1). É um protocolo de transporte sobre um protocolo de transporte; basicamente atualiza o UDP com TLS 1.3.
- Política de **0 RTT** para segundas ligações aos servidores (não precisa de handshake por existência de políticas de perda de pacotes). Primeira ligação: 1 RTT; segunda: 0 RTT (retoma uma ligação anterior).
- **0 RTT ≠ 0 latência**; 0 RTT = menos latência.

### TCP vs QUIC

- **TCP:** uma ordem comum; faz a mudança de ligação por IP.
- **QUIC:** ordem por fluxo; ID de conexão.
- O envio antecipado depende de aceitação e de proteção contra repetição.

### Resumo dos protocolos

- **HTTP** — pedidos e respostas
- **TCP** — fluxo flexível e ordenado
- **UDP** — datagramas sem garantia de entrega
- **TLS** — autenticação e encriptação (3 RTT)
- **QUIC** — transporte seguro com vários fluxos
- HTTP/3 e QUIC são *workarounds* da IETF.
- [[MPLS]] (Multiprotocol Label Switching)
### Estrutura pedida para o trabalho

Funcionamento · Problema · Contexto · Finalidade · História (origem, marco histórico, curiosidade documentada) · Exemplo de utilização.

### Trabalho de aula

**Trabalho 1:** 3 redes, 3 equipamentos por rede, divididas por geografia.

**Trabalho 2 (21/10):** escritório, gabinete do chefe, outro gabinete com 4; desenhar a rede e a tabela de endereçamento. Opcional: rede *guest*.
