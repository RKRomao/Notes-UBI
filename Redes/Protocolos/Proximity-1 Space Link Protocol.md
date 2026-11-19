---
disciplina: Redes
tags:
  - redes
  - espaco
  - enlace
---

# Proximity-1 Space Link Protocol

## O que é e que problema resolve
O **Proximity-1** (CCSDS 211.0-B) define a **camada física e de enlace** para comunicações de **curta distância** entre dois veículos espaciais — por exemplo um *lander* na superfície e um *orbiter*, ou duas naves em *rendezvous*. Não é um protocolo de rede: é o “cabo rádio” fiável entre duas entidades próximas (de metros a alguns milhares de km).

A necessidade: as ligações de espaço profundo usam *links* de baixo débito e RTT enorme; para proximidade quer-se algo mais simples e rápido, com aquisição de ligação, *timing* e retransmissão locais.

## Posição
```
Aplicação (CFDP, imagens, telemetria)
     Proximity-1 (enlace + física)
        Rádio UHF / S-band
```
Pode transportar diretamente PDUs de aplicação ou servir de *bearer* para CFDP/LTP.

## Camadas internas
O Proximity-1 divide-se em:

- **Physical layer (PL)** — modulação, codificação, sincronização de símbolos, *ranging*.
- **Coding & Synchronization Sublayer (C&S)** — *randomization*, código corretor (Reed-Solomon), marcadores de sincronização.
- **Data Link Layer (DLL)** — enquadramento, controlo de acesso ao meio (half-duplex), retransmissão (ARQ), controlo de fluxo.

## Modos de operação
- **Half-duplex**: um lado transmite de cada vez; a ligação é **assimétrica** por natureza.
- **Direct / Relayed**: comunicação direta ou através de um terceiro nó.
- **Sequential / concurrent**: com ou sem canais de comando e telemetria simultâneos.

## Procedimentos de enlace
1. **Aquisição de ligação** — sequência de *handshake* (a *protocol data unit* de sessão) para acordar parâmetros: taxa, codificação, modo.
2. **Transferência de dados** — tramas com número de sequência, ACK/NACK e retransmissão.
3. **Terminação** — fecho ordenado, libertação de recursos.

## Parâmetros negociáveis
- Taxa de dados (ex.: 1 kbps a alguns Mbps, dependendo da versão).
- Codificação (RS, convolucional) e *interleaving*.
- Tamanho de trama.
- Modo de ARQ (com/sem retransmissão).

## Fiabilidade
Fornece **ARQ hop-by-hop** com números de sequência e confirmações — é fiável *entre os dois veículos*, não fim-a-fim (se houver mais saltos, a fiabilidade global é das camadas superiores como CFDP/LTP).

## Casos de uso
- Comunicação rover ↔ orbiter em Marte (o *relay* clássico).
- *Rendezvous* e atracagem automática.
- Transferência de dados entre módulos de uma missão.

## Relação com outros protocolos
- É o *bearer* de **CFDP** e **LTP** em ligações de proximidade.
- Complementa os enlaces de espaço profundo (TM/TC/AOS), que servem distâncias muito maiores.

## Pontos-chave para exame
1. É **física + enlace**, não rede/transporte.
2. Desenhado para **curta distância** e **half-duplex**.
3. Faz **ARQ hop-by-hop**, não fim-a-fim.
4. Negocia parâmetros no *handshake* de aquisição.
5. Serve de suporte a CFDP/LTP.

## Referências
- CCSDS 211.0-B-6 (Proximity-1 Space Link Protocol).
