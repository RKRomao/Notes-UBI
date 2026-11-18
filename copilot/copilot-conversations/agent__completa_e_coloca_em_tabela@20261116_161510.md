---
epoch: 1794845710359
mode: agent
backendId: opencode
sessionId: "ses_e1c00fd50ffewKyyGBZ7e4kjBd"
tags:
  - copilot-conversation
agentLabel: "Completar hierarquia de acesso em tabela"
usage: '{"usedTokens":28729,"contextWindow":200000,"updatedAt":1794845764890}'
---

**user**: completa e coloca em tabela
[Timestamp: 2026/11/16 16:15:10]

**ai**: Atualizei a secção [[Bat (couf couf) Evidence A Wayne]] (substituí a lista indentada por uma tabela RBAC completa). Resultado:

| Nível | Identidade | Certificado | Certificados (PKI/CRL) | Evidências (WORM) | Notas / Pareceres | Quórum (Shamir k=3, n=5) | Âmbito e restrições |
| --- | --- | --- | --- | --- | --- | --- | --- |
| **Root (CA)** | Alfred Pennyworth (HUM-ALFRED-00) | Root CA RSA-4096 (HSM offline) | **C R D** | **C R D** † | R | Inicia a sessão no HSM; custódia das frações | Único emissor da PKI; não deposita provas nem vota |
| **User (Líder)** | Bruce Wayne (BATMAN-01 / BAT-CLI-001) | Leaf Ed25519 | R | **C R** | **C R** | Convoca; fração $x_1$ | Todos os casos que abrir; não apaga provas |
| **User (Terreno)** | Dick Grayson (NIGHTWING-01 / BAT-CLI-002) | Leaf Ed25519 | R | **C R** | **C R** (próprias) | Fração $x_2$ | Casos atribuídos; assinatura na origem |
| **User (Terreno/Forense)** | Tim Drake (ROBIN-01 / BAT-CLI-004) | Leaf Ed25519 | R | **C R** | **C R** (próprias) | Fração $x_4$ | Clonagem, hashing e manifesto antes da submissão |
| **User (Analista)** | Barbara Gordon (ORACLE-01 / BAT-CLI-003) | Leaf Ed25519 | R | **R** + triagem da quarentena | **C R** (pareceres assinados) | Fração $x_3$ | Valida recibos e Merkle; admite provas externas |
| **Extern (Parceiro)** | James Gordon (GCPD-01 / GCPD-CLI-001) | Leaf Ed25519 (6 meses) | — | **C\*** | — | — | Submissão cega; recibo (hash+timestamp); sem leitura |
| **Extern (Instituição)** | GCPD (mailbox) | Credencial de drop-box | — | **C\*** | — | — | Fora da VPN e da BD central; quarentena isolada |

Legenda: **C** criar · **R** ler · **D** eliminar/revogar · **\*** só quarentena.

Decisões que tomei e que deves confirmar:

1. **Separei "Gorden" de "GCPD"** — assumi Gordon = pessoa (GCPD-CLI-001) e GCPD = mailbox institucional, ambas write-only.
2. **`D` do Alfred** — o teu rascunho dava-lhe `w r d` sobre evidências, mas isso contradiz o RNS05/WORM ("escrita e eliminação permanentemente revogadas"). Mantive o `D` com nota de rodapé a limitá-lo a purga de quarentena e revogação de certificados. Se quiseres mesmo admin total, diz e removo a restrição.
3. **Batman perdeu o `D`** que implicitamente teria — nenhum membro pode apagar provas seladas.
4. **Notas** — todos os membros internos podem criar notas próprias; só a Oracle assina pareceres periciais; os externos não leem nem escrevem notas.

Inconsistências que encontrei noutros pontos da nota (não alterei):
- O diagrama "As 3 Cadeias" rotula **Robin como BAT-CLI-002**, mas esse ID é do Nightwing (Robin é BAT-CLI-004).
- O **RF10** refere `AGENT-00` como autoridade central, enquanto [[Personas]] usa `HUM-ALFRED-00`.
- O **RNS08** ainda diz RSA-PSS-2048 para assinaturas, contrariando o perfil criptográfico (Ed25519) definido no início.

Queres que corrija também essas três?
[Timestamp: 2026/11/16 16:15:10]