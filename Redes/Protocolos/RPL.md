---
disciplina: Redes
tags:
  - redes
  - iot
  - routing
---

# RPL — Routing Protocol for Low-Power and Lossy Networks

## O que é e que problema resolve
O **RPL** (RFC 6550) é um protocolo de **roteamento IPv6** para **LLNs** (*Low-power and Lossy Networks*): nós com pouca energia, CPU e memória, ligações instáveis e baixas taxas (802.15.4, PLC, LoRa-ish). O OSPF e o RIP assumem links estáveis e tabelas grandes — não servem.

O RPL constrói **árvores/DAGs** orientadas a um ou mais *sinks*, com métricas que refletem energia, qualidade de link e latência, e adapta-se a mudanças com tráfego de controlo mínimo.

## Estrutura: DODAG
- **DODAG** (*Destination-Oriented DAG*) — grafo acíclico orientado para uma **raiz** (border router / LBR).
- **RPL Instance** — conjunto de DODAGs que partilham política; cada nó escolhe uma instância.
- **DODAGID** — identifica o DODAG (normalmente IPv6 da raiz).
- **Rank** — distância à raiz segundo a *Objective Function*; cresce à medida que se desce.
- **Version** — incrementa quando a topologia é reconstruída.
- **Grounded** — indica se a raiz consegue alcançar a Internet global.

```
        (root/LBR) rank 0
         /     |     \
      rank1  rank1  rank1
       / \      |
   rank2 rank2 rank2 ...
```

## Mensagens de controlo (ICMPv6)
| Mensagem | Direção | Função |
|---|---|---|
| **DIO** | para baixo | anuncia o DODAG, rank, métricas, configuração |
| **DIS** | para cima | pede DIO (descoberta/join) |
| **DAO** | para cima | anuncia prefixos/rotas à raiz |
| **DAO-ACK** | para baixo | confirma DAO (modo *storing*) |

O **Trickle timer** (RFC 6206) controla a frequência de DIO: rápido quando a rede está instável, lento quando está estável — poupança de energia e convergência.

## Objective Functions
A OF define como se calcula o rank e se escolhe o pai:
- **OF0** (RFC 6552) — baseada em *hop count* e métricas de link simples.
- **MRHOF** (RFC 6719) — *Minimum Rank with Hysteresis*; usa **ETX** (expected transmissions) e histerese para evitar oscilações.

Métricas comuns: ETX, energia restante, latência, largura de banda, *link color*.

## Modos de operação
- **Storing (table-driven)** — cada nó guarda rotas para os filhos; DAO propaga rotas para cima. Menos overhead no caminho, mais memória.
- **Non-storing (source routing)** — só a raiz guarda o estado; os pacotes descem com **source routing header** (RFC 6554). Menos memória nos nós, cabeçalhos maiores.

## Tráfego e direções
- **MP2P** (*multipoint-to-point*) — sensores → raiz (caso dominante).
- **P2MP** — raiz → sensores (usa *downward routes*).
- **P2P** — nó ↔ nó; requer descoberta via raiz ou rotas laterais.

## Segurança
- **Secure RPL** com chaves de grupo/par; assinatura de DIO/DAO.
- Proteção contra *rank attacks* (nó anuncia rank falso para atrair tráfego), *sinkhole*, *blackhole*, *wormhole*.
- Associação segura ao DODAG (chaves pré-configuradas ou negociadas).
- Em 6TiSCH, integra-se com a segurança do 802.15.4.

## Comparação
| | RPL | OSPF | AODV |
|---|---|---|---|
| Tipo | proativo, hierárquico | proativo, link-state | reativo |
| Alvo | LLN | redes estáveis | MANET |
| Métrica | ETX/energia | custo | hops |
| Estado | por nó (storing) | LSDB | rotas on-demand |

## Casos de uso
- **6LoWPAN / 6TiSCH** — RPL é o protocolo de rede padrão.
- Smart metering (DLMS/COSEM sobre RPL).
- Automação predial e industrial.
- Cidades inteligentes.

## Pontos-chave para exame
1. RPL constrói **DODAGs** orientados a uma raiz.
2. **DIO/DIS/DAO/DAO-ACK** são o controlo (ICMPv6).
3. **Rank** + **Objective Function** (OF0, MRHOF/ETX).
4. **Storing vs non-storing**; **Trickle** poupa energia.
5. É o protocolo de rede do **6LoWPAN/6TiSCH**.

## Referências
- RFC 6550 (RPL), RFC 6552 (OF0), RFC 6719 (MRHOF), RFC 6206 (Trickle), RFC 6554 (source routing).
