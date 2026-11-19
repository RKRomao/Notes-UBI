---
disciplina: Redes
tags:
  - redes
  - routing
  - arquitectura
---

# LISP — Locator/ID Separation Protocol

## O que é e que problema resolve
O **LISP** (RFC 9300/9301) ataca um defeito estrutural do IP: **um endereço faz dois papéis ao mesmo tempo** — identifica o *host* (quem é) e localiza-o na topologia (onde está). Quando um host se move ou uma rede tem múltiplos fornecedores, o prefixo tem de mudar ou ser anunciado por todos os lados, o que faz crescer a tabela global de BGP e quebra sessões.

O LISP **separa**:

- **EID (Endpoint Identifier)** — identidade estável do host/rede (ex.: `192.168.1.10`);
- **RLOC (Routing Locator)** — endereço topológico do router que serve o EID (ex.: IP público do xTR).

O *core* da Internet só encaminha **RLOCs**; os EIDs vivem nas *edge* networks e são resolvidos por um **sistema de mapeamento**.

## Entidades
| Entidade | Papel |
|---|---|
| **ITR** (Ingress Tunnel Router) | encapsula pacotes EID→RLOC |
| **ETR** (Egress Tunnel Router) | desencapsula e entrega ao EID |
| **xTR** | ITR + ETR no mesmo router |
| **Map-Server (MS)** | guarda registos EID→RLOC |
| **Map-Resolver (MR)** | responde a *Map-Requests* (pode consultar MS/ALT) |
| **PITR / PETR** | *proxy* tunnel routers para redes não-LISP |

## Plano de dados
O ITR encapsula o pacote original num **túnel LISP**:

```
[IP exterior: RLOC origem → RLOC destino][UDP 4341][LISP header][pacote original EID→EID]
```

- UDP porta **4341**.
- O header LISP tem *nonce*, flags e, opcionalmente, *instance ID* (VRF).
- O ETR desencapsula e entrega ao EID de destino.
- Suporta **IPv4-in-IPv6**, **IPv6-in-IPv4**, etc. (*address family traversal*).

## Plano de controlo
- O ETR **registra** os seus EIDs num Map-Server (**Map-Register**, UDP 4342).
- O ITR, ao ver tráfego para um EID desconhecido, envia **Map-Request** ao Map-Resolver.
- Recebe **Map-Reply** com a lista de RLOCs e **prioridades/pesos**.
- Pode usar **pub/sub** (Map-Notify) para atualizações.

As prioridades/pesos permitem **multihoming ativo-ativo** e *traffic engineering*: o ITR escolhe o RLOC conforme política.

## Vantagens
- **Multihoming** sem anunciar prefixos ao core.
- **Mobilidade** de VMs/hosts sem mudar EID → sessões sobrevivem.
- **Redução da tabela global** de rotas (só RLOCs no core).
- **Traffic engineering** por escolha de RLOC.
- **Encaminhamento por política** e segmentação (instance IDs).

## Desvantagens / desafios
- **Overlay**: overhead de encapsulamento e possível *stretch* (caminho não ótimo).
- Depende de **mapeamento**: latência de resolução, cache, disponibilidade do MS/MR.
- **MTU** reduzida pelo header extra → necessidade de fragmentação/PMTUD.
- Complexidade operacional e de diagnóstico (túneis escondem a topologia).
- Segurança do mapeamento (autenticação de registos) é crítica.

## Segurança
- Autenticação de Map-Register (chaves pré-partilhadas, HMAC).
- Separação de instâncias (VRF) impede fuga entre clientes.
- O encapsulamento pode ser combinado com IPsec.
- Riscos: *spoofing* de EID/RLOC, envenenamento de mapeamentos, *cache poisoning*.

## Comparação
| | LISP | BGP clássico | MPLS VPN |
|---|---|---|---|
| Separa ID/local | sim | não | parcial (VRF) |
| Core carrega | RLOCs | prefixos dos clientes | labels |
| Mobilidade | nativa | difícil | limitada |
| Overhead | encapsulamento | nenhum | label |

## Casos de uso
- Empresas multi-homed que não querem anunciar prefixos.
- Data centers com migração de VMs.
- Operadores com *traffic engineering* e segmentação.
- Investigação: arquiteturas *future Internet*.

## Pontos-chave para exame
1. LISP separa **EID** (identidade) de **RLOC** (localização).
2. Data plane: **encapsulamento UDP 4341**; control plane: **4342**.
3. Entidades: **ITR/ETR/xTR, Map-Server, Map-Resolver**.
4. Benefícios: **multihoming, mobilidade, tabela global menor**.
5. Custos: **overhead, MTU, dependência do mapeamento**.

## Referências
- RFC 9300 (LISP data plane), RFC 9301 (control plane), RFC 6830 (original).
