---
disciplina: Redes
tags:
  - redes
  - iot
  - mac
---

# TSCH — Time Slotted Channel Hopping

## O que é e que problema resolve
O **TSCH** é um modo de operação da camada MAC do **IEEE 802.15.4** (introduzido no 802.15.4e-2012, integrado no 802.15.4-2015). Combina duas ideias:

- **TDMA** — o tempo é dividido em *slots* e cada nó transmite só na sua janela;
- **Channel hopping** — cada slot usa um canal diferente segundo uma sequência.

Resolve dois problemas típicos de redes industriais sem fios:

1. **Interferência** — saltar de canal evita *fading* e interferência persistente num canal.
2. **Determinismo e consumo** — slots fixos permitem *duty cycling* agressivo e latência previsível.

É a base de **WirelessHART**, **ISA100.11a** e **6TiSCH** (RFC 8180).

## Conceitos
- **Slot** (timeslot) — unidade de tempo, tipicamente **10 ms** (pode variar).
- **Slotframe** — sequência repetida de slots; define o período de repetição.
- **Cell** (célula) — par **(slot, canal)** que pode ser atribuído a um par de nós. Uma célula é um “recurso de transmissão”.
- **ASN (Absolute Slot Number)** — contador global de slots desde o arranque da rede; usado para derivar canal e sincronização.
- **Link schedule** — matriz de células atribuídas (dedicadas ou partilhadas).

```
Slotframe (ex.: 101 slots × 10 ms ≈ 1 s)
┌──┬──┬──┬── ... ──┬──┐
│s0│s1│s2│        │s100│
└──┴──┴──┴────────┴──┘
cada slot → canal = f(ASN)
```

## Sincronização
Os nós sincronizam-se por **Enhanced Beacons (EB)** que transportam:

- **ASN**;
- *slotframe* e *timeslot*;
- **hopping sequence ID** e *channel offset*.

A partir do ASN e da sequência, cada nó calcula o canal do slot:

```
canal = hopping_sequence[(ASN + channel_offset) mod N]
```

Sem sincronização não há TSCH — o *join* é feito ouvindo EBs.

## Agendamento
O schedule pode ser:

- **Estático** — células pré-atribuídas (simples, pouco adaptativo);
- **Dinâmico** — negociado conforme tráfego (ex.: **6P**, RFC 8480; ou MSF — *Minimal Scheduling Function*, RFC 9033).

Células **dedicadas** evitam colisões; **partilhadas** (CSMA dentro do slot) servem tráfego esporádico.

## Consumo e latência
- Rádio ligado apenas no slot atribuído → *duty cycle* < 1 %.
- Latência **determinística** (máximo = período do slotframe × hops).
- *Channel hopping* melhora a robustez sem aumentar potência.

## Relação com 6TiSCH
O **6TiSCH** junta:
- **802.15.4 TSCH** (MAC),
- **6LoWPAN** (adaptação IPv6),
- **RPL** (roteamento),
- **CoAP** (aplicação),
- e uma *Scheduling Function* (6P/MSF) para gerir células.

É a arquitetura de referência para **IoT industrial IPv6 determinístico**.

## Segurança
- Segurança do 802.15.4 (AES-CCM*) na MAC.
- Chaves de rede e de *join*; proteção de EBs.
- TSCH em si não define esquema de chaves — herda do 802.15.4 e do perfil (ex.: 6TiSCH minimal security, RFC 8180).

## Comparação
| | TSCH | 802.15.4 CSMA/CA | BLE |
|---|---|---|---|
| Acesso | TDMA + hopping | contention | TDMA-ish |
| Determinismo | alto | baixo | médio |
| Consumo | muito baixo | baixo | baixo |
| Interferência | robusta (hopping) | vulnerável | adaptativo |

## Casos de uso
- Automação industrial e fábricas.
- Smart grid / medição.
- Edifícios inteligentes, sensores críticos.

## Pontos-chave para exame
1. TSCH = **TDMA + channel hopping** no 802.15.4.
2. **Slot / slotframe / cell / ASN** são os conceitos-base.
3. Sincronização por **Enhanced Beacons**; canal derivado do **ASN**.
4. Base do **6TiSCH**, WirelessHART e ISA100.11a.
5. Ganhos: **determinismo, baixo consumo, robustez à interferência**.

## Referências
- IEEE 802.15.4-2015 (TSCH), IEEE 802.15.4e-2012.
- RFC 8180 (6TiSCH), RFC 8480 (6P), RFC 9033 (MSF).
