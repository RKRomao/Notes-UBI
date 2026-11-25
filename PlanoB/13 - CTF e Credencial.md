---
tags:
  - planb
  - planb/tema-13
tema: 13
---

# Tema 13: Desafio Criptográfico (CTF Captive Portal & Credencial Hacker)

[[12 - Terminal CLI|← Anterior]] | [[Plan B|Índice]] | [[14 - Catálogo Visual|Próximo →]]

Para manter a fidelidade à cultura hacker e filtrar o acesso a descontos exclusivos nos workshops dos bastidores, o portal web implementa um mini-CTF client-side estático (compatível com GitHub Pages):

```mermaid
sequenceDiagram
autonumber
actor Dev as Utilizador / Hacker
participant Web as Portal Web / Console
participant Crypto as Web Crypto API (SHA-256)
participant Modal as Credencial Aluno #PB-1337

Dev->>Web: Inspeciona robots.txt, cookies ou executa 'wifi' no terminal
Web-->>Dev: Retorna Token Cifrado: YzQwX3EzX3kwX2Zyel9lcnB1cnZiXzEzMzc=
Note over Dev: Etapa 1: Base64 Decode -> c40_q3_y0_frz_erpurvb_1337
Note over Dev: Etapa 2: Decifração ROT13 -> p40_d3_l0_sem_recheio_1337
Dev->>Web: Submete: FLAG{p40_d3_l0_sem_recheio_1337}
Web->>Crypto: Gera hash SHA-256 da string
Crypto-->>Web: Confirma hash 836c0b5cbf8b...400
Web->>Modal: Desbloqueia Modal com Áudio Web Audio API
Modal-->>Dev: Exibe Credencial com Voucher FLAG-BASTIDORES-1337 (-50%)
```
