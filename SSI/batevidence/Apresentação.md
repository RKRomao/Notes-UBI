## Contexto e Motivação

O BatEvidence é um sistema de catalogação e gestão de provas forenses digitais, desenvolvido para garantir a **cadeia de custódia ininterrupta** e a **integridade absoluta** das evidências recolhidas no terreno. Os utilizadores são membros da BatFamily, e as provas podem ser depositadas enquanto os agentes operam no terreno, em condições de rede instável.

## Objetivos

- Rastreabilidade completa de cada evidência (quem depositou, quando, em que caso).
- Integridade e autenticidade verificáveis a qualquer momento.
- Confidencialidade, integridade e autenticidade das comunicações.
- Armazenamento de evidências sensíveis que só se revelam com um quórum predefinido (Shamir).
- Evidência original imutável (WORM); anotações guardadas separadamente, encadeadas.

## Atores (Personas)

- **Alfred Pennyworth** -- Root CA / HSM offline.
- **Bruce Wayne (Batman)** -- líder; convoca quóruns, deposita provas.
- **Dick Grayson (Nightwing)**, **Tim Drake (Robin)** -- operadores de terreno.
- **Barbara Gordon (Oracle)** -- analista; triagem, validação, anotações.
- **Comissário James Gordon (GCPD)** -- parceiro externo, drop-box write-only.

## Cenários-Chave

1. **Recolha e depósito em campo** -- clonagem, hash SHA-256, assinatura, submissão capturada/analizada com Wireshark; recibo assinado.
2. **Abertura condicionada** -- quórum Shamir (k-de-n) com HSM do Alfred; auditoria total.
3. **Análise e anotação** -- validação de recibos e Merkle Tree; anotações encadeadas, sem tocar na prova.
4. **Submissão externa (GCPD)** -- drop-box write-only para quarentena, triagem pela Oracle.

## Perfil Criptográfico

| Elemento | Algoritmo | Justificação |
|---|---|---|
| Root CA (Alfred, HSM offline) | RSA-4096 (RSA-PSS) | Vida longa (10 anos), compatibilidade PKI |
| Certificados de folha | Ed25519 | Handshakes frequentes, chaves de 32 bytes |
| Parceiros externos | Ed25519 (RSA-PSS ≥3072 se exigido) | Interoperabilidade |
| Assinatura de provas/notas/blocos | Ed25519 | Ledger compacto, dispositivos de campo |
| Verificação | Lista de algoritmos permitidos | Impede downgrade |

Regra: 
Ed25519 por omissão; RSA-PSS apenas na raiz e em parceiros que o exijam.

## Arquitetura

- **Entrada:** Membro → App Cliente → Autenticação Wireshark/WireGuard → Casos.
- **Cada caso:** Evidências WORM + Notas Encadeadas (separação física).
- **Cross-reference:** uma prova pode ser referenciada por vários casos.

## Base de Dados Híbrida

- **Nó Relacional (SQL):** utilizadores, metadados de casos, partes Shamir cifradas.
- **Nós Blockchain (Ledger):** verdade jurídica; apenas INSERT se Prev_Hash bater certo. Dois ledgers: `T_CASE_BLOCK` e `T_AUDIT_BLOCK`.
- **Nó Object Storage (WORM):** payload cifrado AES-256, imutável (Object Lock).

## Fluxos Principais

### Acesso Seguro (Consulta)
Pedido autenticado → recuperação e verificação (Merkle) → quórum e enclave (reconstrução AES, re-cifragem com chave pública do cliente, E2EE contra o backend) → entrega e descifragem local.

### Criação e Depósito
Fase 1: abertura do caso e chaves (Shamir). Fase 2: preparação off-chain (hash, cifra AES, zeroing). Fase 3: ancoragem on-chain (Bloco 1). Fase 4: encerramento (bloco terminal, lock).

## Três Cadeias Interligadas

1. **PKI / Trust Chain** — Alfred (Root CA, RSA-4096) emite folhas Ed25519; CRL em tempo real.
2. **Case Ledger** — sub-cadeia por caso (Bloco 0 Genesis → Bloco 1 Prova → Bloco 2 Anotação → Bloco 3 Terminal).
3. **Global Audit Sidechain** — logs de sistema (boot, Wireshark, quórum, wipe do enclave).

Num único depósito de prova, as três cadeias interagem: a folha certifica a identidade, a submissão gera log de auditoria, e a prova entra no ledger do caso.

## Pontos de Demonstração

Este trabalho é um plano de segurança; não inclui demonstrações práticas. Os pontos a apresentar são a análise e o desenho dos mecanismos (PKI, quórum Shamir, WORM, E2EE) e a sua justificação face aos requisitos forenses.

## Conclusão

O BatEvidence combina PKI, blockchain, WORM storage, Shamir e E2EE para garantir cadeia de custódia, não-repúdio e quórum de acesso, cobrindo os requisitos forenses e de confidencialidade do sistema.
