---
disciplina: Redes
tags:
  - redes
  - fundamentos
---
# Pilha TCP/IP

A pilha TCP/IP é o modelo em camadas que sustenta a Internet. Cada camada presta um serviço à camada superior e usa os serviços da camada inferior.

| Camada            | Função                                    | Exemplos                                               |
| ----------------- | ----------------------------------------- | ------------------------------------------------------ |
| **Aplicação**     | Protocolos usados pelos programas         | HTTP, DNS, SMTP                                        |
| **Transporte**    | Comunicação fim-a-fim entre processos     | [[Camada de Transporte\|TCP]], UDP, [[QUIC]], [[SCTP]] |
| **Internet**      | Endereçamento e encaminhamento de pacotes | IP, [[OSPF]], ICMP                                     |
| **Acesso à rede** | Ligação física e enlace                   | Ethernet, Wi-Fi                                        |

## A camada de transporte

> [!important] A camada responsável pela **fiabilidade** é a **camada de transporte**.

É a camada de transporte que garante (ou não) que os dados chegam completos, pela ordem certa e sem duplicações. Para isso usa mecanismos como:

- **Confirmações (ACK / NACK)** : o recetor informa se recebeu, ou não, os dados.
- **Checksum** : deteção de erros de bits no segmento.
- **Timeout e retransmissão**  : se a confirmação não chega dentro do tempo limite, o emissor reenvia.
- **Controlo de fluxo e de congestionamento** : evita saturar o recetor ou a rede.

Ver em detalhe: [[Camada de Transporte]].

## Protocolos em comparação

| Protocolo | Camada | O que faz |
| --- | --- | --- |
| **HTTP** | Aplicação | Pedidos e respostas (cliente ↔ servidor) |
| **TCP** | Transporte | Fluxo fiável e ordenado |
| **UDP** | Transporte | Datagramas, sem garantia de entrega |
| **TLS** | Entre transporte e aplicação | Autenticação e encriptação |
| **QUIC** | Transporte (sobre UDP) | Transporte seguro com vários fluxos independentes |

> [!note] HTTP/3 e QUIC nasceram como *workarounds* da IETF para os problemas de desempenho do TCP.

## Ligações

- [[HTTP]] — a evolução do protocolo e o problema de *head-of-line blocking*
- [[QUIC]] — a solução adotada no HTTP/3
- [[SCTP]] — o terceiro protocolo de transporte da pilha
- [[OSPF]] — encaminhamento dinâmico na camada de Internet
- [[NFC]] —  comunicação em meio Contactless 
- [[Trabalhos e Avaliação]]
