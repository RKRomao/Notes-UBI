---
epoch: 1794826499494
mode: agent
backendId: opencode
sessionId: ses_e979c553cffelJ0o5boVH8DFWw
agentLabel: Organização das notas de Redes
tags:
  - copilot-conversation
lastAccessedAt: 1794845686519
---

**user**: organisa as notas da disciplina de redes
[Timestamp: Unknown time]

**ai**: 
[Timestamp: Unknown time]

**user**: organisa as notas da disciplina de redes
[Timestamp: Unknown time]

**ai**: Organizei a disciplina numa pasta `Redes/` e transformei o [[Redes]] num índice (MOC). O conteúdo solto das aulas foi separado por tema e desenvolvido, sem perder o registo original.

**Estrutura criada**

| Nota | Conteúdo |
| --- | --- |
| [[Redes]] | Índice, mapa das matérias, ideias-chave e caminho de estudo |
| [[Pilha TCP-IP]] | Camadas, papel da camada de transporte e tabela comparativa dos protocolos |
| [[Camada de Transporte]] | ACK/NACK, checksum, timeout, TCP vs UDP, portas e HOL blocking |
| [[HTTP]] | Evolução 1.0 → 1.1 → 2 → 3 e o problema do head-of-line blocking |
| [[QUIC]] | UDP + TLS 1.3, fluxos independentes, connection ID, 0-RTT |
| [[SCTP]] | Multi-homing, multi-streaming, história, 4-way handshake e exemplos |
| [[OSPF]] | Link-state, Dijkstra, história/RFCs e exemplo Porto–Lisboa–Coimbra |
| [[Trabalhos e Avaliação]] | Avisos para o teste e enunciados dos trabalhos 1 e 2 |
| [[Notas de aula (originais)]] | Registo em bruto das aulas, como arquivo |

**O que fiz com o texto original**
- Corrigi os apontamentos rápidos («agnol» → ACK/NACK, «cmada» → camada, «headoflineblocking» → *head-of-line blocking*).
- Mantive as secções SCTP/OSPF praticamente intactas, apenas com formatação e tabelas.
- Marquei os pontos assinalados como avisos de teste em callouts (`> [!warning]`).
- Acrescentei detalhe técnico onde faltava: handshake TCP/TLS e os ~3 RTT no HTTP/2 clássico; RFC 2328 do OSPF; fórmulas de custo.
- Preservei tudo o que estava em bruto na nota de arquivo, para não se perder nenhuma referência das aulas.

**Notas de interpretação**
- «AVS» foi tratado como *avisos* (temas para o teste).
- «três RTT» ficou associado a TCP + TLS 1.2, que é o cenário em que o valor bate certo; com TLS 1.3 são 2 RTT no total.

Se quiseres, posso agora desenhar o diagrama da rede do trabalho 2 (com tabela de endereçamento) ou criar um canvas com o mapa dos protocolos.
[Timestamp: Unknown time]

**user**: faz par a o NFC o que foi feito para os outros protocolos
[Timestamp: Unknown time]

**ai**: Criei [[NFC]] com a mesma estrutura das notas de [[SCTP]] e [[OSPF]] e integrei-o no índice [[Redes]].

**O que a nota inclui**
- **Funcionalidade** — 13,56 MHz, alcance ~4 cm, débitos 106/212/424 kbit/s, acoplamento indutivo (tags sem bateria), modos passivo/ativo e os três modos de operação: emulação de cartão, leitor/escritor e *peer-to-peer*.
- **Problema** — substituição de códigos de barras/QR, fim do emparelhamento manual de Bluetooth/Wi-Fi, pagamentos e acessos mais rápidos, tags sem bateria e proximidade como fator de segurança.
- **Contexto** — pagamentos *contactless*, transportes, controlo de acessos, *smart posters*, IoT e *pairing*.
- **Finalidade** — troca simples e segura de pequenos volumes de dados, servindo de ponte para outras tecnologias.
- **História** — origem no RFID HF/MIFARE/FeliCa; Philips e Sony em 2002; ISO/IEC 18092 em 2003; NFC Forum em 2004; Android Beam (2011) e Apple Pay (2014); curiosidades (NFC ≠ RFID, NDEF, LLCP, alcance curto intencional).
- **Exemplo de utilização** — pagamento *contactless* passo a passo e aprovisionamento de Wi-Fi.
- **Tabela comparativa** NFC vs RFID HF vs Bluetooth.

**Integração**
- Acrescentei a secção «Tecnologias de proximidade» e uma ideia-chave no [[Redes]].
- O `NFC.md` vazio da raiz foi removido; a nota passou para `Redes/NFC.md`, evitando dois ficheiros com o mesmo nome.
- Referências verificadas: ISO/IEC 18092 (dez. 2003), NFC Forum (2004), 13,56 MHz e débitos até 424 kbit/s.

Nota: classifiquei o NFC como tecnologia de proximidade e não como protocolo da pilha TCP/IP, porque atua nas camadas de acesso à rede/aplicação. Se na tua disciplina ele for apresentado como protocolo de transporte, digo-me e ajusto a classificação.
[Timestamp: Unknown time]