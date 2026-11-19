---
disciplina: Redes
tags:
  - redes
  - routing
  - operador
---

# MPLS — Multi-Protocol Label Switching

## O que é e que problema resolve
O **MPLS** (RFC 3031) substitui o encaminhamento “hop-by-hop por prefixo IP” por **comutação por etiquetas**: um router de entrada classifica o pacote, atribui uma **label**, e os routers seguintes apenas trocam labels — sem analisar o cabeçalho IP.

Nasceu para resolver:
- **velocidade** (nos anos 90, lookup de label era mais rápido que longest-prefix match — hoje é irrelevante);
- **traffic engineering** — encaminhar por caminhos que o IGP não escolheria;
- **VPNs** escaláveis — separar clientes por labels em vez de túneis;
- **multiprotocolo** — transportar IP, Ethernet, ATM, etc.

## Conceitos
- **FEC (Forwarding Equivalence Class)** — conjunto de pacotes tratados da mesma forma (ex.: um prefixo).
- **Label** — identificador local com significado por salto.
- **LSR (Label Switching Router)** — router que comuta labels.
- **LER (Label Edge Router)** — entrada/saída do domínio; impõe/remove labels.
- **LSP (Label Switched Path)** — caminho estabelecido para um FEC.
- **PHP (Penultimate Hop Popping)** — o penúltimo router remove a label para poupar um lookup ao último.

## Cabeçalho (shim header)
32 bits, inserido entre L2 e L3:

```
| Label (20) | EXP/TC (3) | S (1) | TTL (8) |
```

- **Label** — 20 bits (0–15 reservados: 0 = IPv4 explicit null, 3 = implicit null, 14 = OAM).
- **EXP/TC** — classe de serviço / QoS.
- **S** — *bottom of stack* (1 = última label).
- **TTL** — evita loops.

As labels podem ser **empilhadas** (label stack) → útil para VPN e *tunneling*.

## Distribuição de labels
- **LDP** (RFC 5036) — protocolo simples, segue o IGP; mapeia prefixos em labels.
- **RSVP-TE** (RFC 3209) — reserva recursos e estabelece LSPs explícitos (traffic engineering).
- **BGP** — usado para **L3VPN** (labels de VPNv4/VPNv6).
- **SR (Segment Routing)** — evolução: labels como *segments* (SR-MPLS), sem LDP/RSVP.

## Funcionamento
1. O LER de entrada classifica o pacote num FEC e empilha a label correspondente.
2. Cada LSR consulta a **ILM** (*Incoming Label Map*): troca label e escolhe interface/next-hop.
3. O LER de saída remove a label e entrega o pacote IP.

## MPLS e VPNs
- **VRF** por cliente no PE (Provider Edge).
- **RD (Route Distinguisher)** torna prefixos únicos entre clientes.
- **RT (Route Target)** controla import/export de rotas.
- **L3VPN** (RFC 4364): o PE empilha label de transporte + label de VPN.
- **L2VPN / VPLS / EVPN**: transporte de Ethernet sobre MPLS.

## Traffic Engineering e resiliência
- **TE** — LSPs explícitos com largura de banda reservada, contornando congestão.
- **Fast Reroute (FRR)** — proteção local (<50 ms) com *backup LSP* ou *facility backup*.
- **MPLS-TP** — perfil para transporte (OAM, proteção, sem IP no plano de dados).

## QoS
O campo **EXP/TC** transporta a classe; permite **DiffServ sobre MPLS** com filas e policing por label.

## Vantagens
- Engenharia de tráfego e caminhos determinísticos.
- VPNs escaláveis e multi-serviço.
- Recuperação rápida de falhas.
- Convergência de redes (IP + Ethernet + transporte).

## Limitações
- Complexidade de operação e troubleshooting.
- Estado por LSP nos routers (RSVP-TE).
- Overhead de 4 bytes por label.
- Concorrência do **Segment Routing** (mais simples, sem LDP/RSVP).

## Comparação
| | MPLS | IP clássico | Segment Routing |
|---|---|---|---|
| Encaminhamento | label | prefixo | label/segmento |
| TE | RSVP-TE | difícil | nativo |
| Estado | por LSP | por prefixo | reduzido |
| VPN | L3VPN/L2VPN | túneis | EVPN/SR |

## Casos de uso
- Backbone de operadores.
- VPNs empresariais L2/L3.
- Transporte de tráfego de data centers (EVPN-MPLS).
- Redes de energia/transportes com MPLS-TP.

## Pontos-chave para exame
1. MPLS = **comutação por labels**, separa encaminhamento de análise IP.
2. **Shim header**: label(20) + EXP(3) + S(1) + TTL(8).
3. **LER vs LSR**, **LSP**, **PHP**, **FEC**.
4. Distribuição por **LDP / RSVP-TE / BGP**.
5. **L3VPN** usa RD/RT + label stack.
6. **TE** e **FRR** são as grandes vantagens operacionais.

## Referências
- RFC 3031 (arquitetura MPLS), RFC 5036 (LDP), RFC 3209 (RSVP-TE), RFC 4364 (L3VPN).
