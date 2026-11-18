---
epoch: 1794915568969
mode: agent
backendId: opencode
sessionId: "ses_e16b238adffeWtL6yJLUGeFozO"
tags:
  - copilot-conversation
agentLabel: "WireGuard compatível com satélite"
usage: '{"usedTokens":29872,"contextWindow":1048576,"updatedAt":1794922362109}'
lastAccessedAt: 1794921141815
---

**user**: wiregard e compativel com arquiteturas de satelite?
[Timestamp: 2026/11/17 11:39:28]

**ai**: Sim — o WireGuard é **compatível** com arquiteturas de satélite (é usado sobre satélite hoje em dia), mas **nem sempre performa bem**. A compatibilidade técnica existe; o problema é de desempenho em links de alta latência e perda.

Pontos-chave da literatura:

- **WireGuard (UDP) é geralmente o melhor VPN para satélite** em comparação com OpenVPN TCP/IPsec, por ser leve e ter handshake de 1 round-trip. Fontes (Secure Tools Guide, vpn.how) recomendam WireGuard como padrão para links GEO/LEO, com MTU ~1280–1420, MSS 1200–1300, BBR v2 e keepalive de 20–30s.
- **GEO (600–800 ms RTT):** funciona, mas com tuning — MTU mais baixo, buffers grandes, FEC opcional. A aceleração TCP do modem satélite **não enxerga** o fluxo TCP dentro do túnel UDP do WireGuard, então TCP dentro do túnel pode ser lento (relato de 1/30 da performance em GEO).
- **LEO (Starlink, 30–70 ms):** quase qualidade de rede móvel; WireGuard funciona muito bem.
- **Certus (Iridium, L-band):** VPNs não são recomendadas; WireGuard TCP teve 4% de perda e outages de ~2 min em testes. Alguns provedores oferecem VPNs otimizadas para Certus.
- **Ponto crítico:** o PEP (Performance Enhancing Proxy) do satélite acelera TCP em claro; encapsulado em UDP pelo WireGuard, essa aceleração é perdida. A recomendação técnica (paper VIVERIS/CNES) é usar WireGuard com PEP na **posição B** (não A) para evitar "TCP-in-TCP".

Resumo: compatível e recomendado para a maioria dos casos (especialmente LEO e GEO com tuning), mas em GEO com TCP dentro do túnel ou em satélites L-band (Certus) o desempenho degrada e pode ser necessário fallback para OpenVPN TCP ou VPN otimizada do provedor.
[Timestamp: 2026/11/17 11:39:28]

**user**: coloca isso nas notas do trabalho do bat (couf couf)
[Timestamp: 2026/11/17 13:12:59]

**ai**: Adicionado à secção **Arquitetura Física** da nota [[Bat (couf couf) Evidence A Wayne]], logo abaixo da linha que menciona o acesso por satélite da Wayne Enterprises, uma nota sobre a compatibilidade e desempenho do WireGuard em arquiteturas de satélite (LEO/GEO/L-band, PEP, tuning recomendado).
[Timestamp: 2026/11/17 13:12:59]

**user**: completa com descricoes mais completas e esquemas em mermead
[Timestamp: 2026/11/17 13:32:29]

**ai**: Completo: adicionei à nota [[Bat (couf couf) Evidence A Wayne]] uma subsecção detalhada sobre WireGuard em arquiteturas de satélite — tabela por órbita (LEO/GEO/L-band), explicação do problema do PEP, configuração recomendada e dois diagramas Mermaid (fluxo de desempenho e sequência de encapsulamento).
[Timestamp: 2026/11/17 13:32:29]