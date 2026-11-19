---
disciplina: Redes
tags:
  - redes
  - iot
  - lpwan
---

# LoRaWAN — Long Range Wide Area Network

## O que é e que problema resolve
O **LoRaWAN** é um protocolo de **rede LPWAN** (Low-Power Wide-Area Network) que corre sobre a modulação física **LoRa**. Foi criado pela Semtech/LoRa Alliance para ligar sensores a **quilómetros de distância** com **anos de bateria** e débitos muito baixos (0,3–50 kbps). Não serve para vídeo nem voz — serve para telemetria esporádica: contadores, sensores ambientais, rastreio.

O truque físico é **CSS (Chirp Spread Spectrum)**: *chirps* de frequência que resistem a ruído abaixo do piso de ruído (*processing gain*), com trade-off direto entre **alcance**, **débito** e **consumo** via **Spreading Factor (SF7–SF12)**.

## Arquitetura
```
End device ──(LoRa)──► Gateway ──(IP)──► Network Server ──► Application Server
                                              │
                                         Join Server
```

- **End device** — sensor/atuador com rádio LoRa.
- **Gateway** — recebe LoRa e reencaminha por IP (backhaul celular/Ethernet); é “burro”, faz *packet forwarder*.
- **Network Server (NS)** — deduplicação, gestão de sessões, MAC, ADR, agendamento de downlink.
- **Join Server (JS)** — gestão de chaves de ativação.
- **Application Server (AS)** — lógica de negócio.

Topologia em **estrela de estrelas**: dispositivos não falam entre si; falam com gateways.

## Classes de dispositivo
| Classe | Comportamento | Latência downlink | Consumo |
|---|---|---|---|
| **A** | 2 janelas de receção após cada uplink; resto dorme | alta | mínimo |
| **B** | A + *beacon* periódico → *ping slots* agendados | média | médio |
| **C** | receção contínua quando não transmite | mínima | máximo |

A **Classe A** é obrigatória e a base; B e C são extensões.

## Ativação e segurança
- **OTAA** (*Over-The-Air Activation*) — JoinEUI/DevEUI/AppKey → deriva **NwkSKey** e **AppSKey**. Recomendada; permite *roaming* e renovação.
- **ABP** (*Activation By Personalization*) — chaves pré-configuradas; simples mas rígida e menos segura.

Cifra em duas camadas:
- **NwkSKey** — integridade e cifra do MAC (MIC, payload entre device e NS);
- **AppSKey** — cifra **ponta-a-ponta** até ao Application Server.
Algoritmo: **AES-128** (CMAC para MIC, CTR para payload). Contadores de frame evitam *replay*.

## Camadas e formato
- **PHY**: LoRa CSS; bandas ISM (EU868, US915, AS923, …); canais e *duty cycle* regionais.
- **MAC**: LoRaWAN (LoRa Alliance); tipos de mensagem Join-Request/Accept, Data Up/Down, MAC commands.
- **Payload** máximo depende do *Data Rate*: ~51 bytes em SF12 até ~242 bytes em SF7 (EU868).
- **Duty cycle**: na EU868, 1 % por sub-banda (limite regulamentar) — condiciona o débito efetivo.

## ADR — Adaptive Data Rate
O Network Server ajusta **SF, potência e largura de banda** por dispositivo:
- link bom → SF baixo → mais débito, menos tempo no ar, menos energia;
- link mau → SF alto → mais alcance, mais tempo no ar.
Dispositivos móveis podem desativar ADR.

## Vantagens
- Alcance de **2–15 km** (rural) e **~2 km** (urbano).
- Bateria de **5–10 anos**.
- Gateways baratos e implantação simples (unlicensed).
- Cifra AES-128 ponta-a-ponta.

## Limitações
- Débito muito baixo e *duty cycle* limitado.
- Downlink escasso (Classe A é assimétrica).
- **Sem QoS** nem fiabilidade forte (confirmações opcionais, limitadas).
- Interferência em bandas ISM partilhadas.
- Escalabilidade de downlink é o gargalo em redes densas.

## Comparação
| | LoRaWAN | NB-IoT | Sigfox | Zigbee |
|---|---|---|---|---|
| Espectro | ISM | licenciado LTE | ISM | ISM |
| Alcance | km | km | km | ~100 m |
| Débito | baixo | médio | muito baixo | médio |
| Bateria | anos | anos | anos | meses-anos |
| Topologia | estrela | celular | estrela | mesh |

## Casos de uso
- Contadores de água/gás/eletricidade.
- Agricultura de precisão, humidade/temperatura.
- Rastreio de ativos e logística.
- Cidades inteligentes (contentores, parques, ruído).

## Pontos-chave para exame
1. **LoRa = física (CSS)**; **LoRaWAN = MAC/rede**.
2. Topologia **estrela de estrelas** com gateways “burros”.
3. **Classes A (base), B e C**.
4. **OTAA vs ABP**; **NwkSKey** (MAC) e **AppSKey** (E2E).
5. **ADR** troca alcance por débito/energia.
6. *Duty cycle* e payload pequeno são as limitações centrais.

## Referências
- LoRa Alliance, *LoRaWAN Specification* 1.0.x / 1.1.
- Regulamentação regional (ETSI EN 300 220 para EU868).
