# Apresentação — BatEvidence (SSI)

> **Duração:** 10 minutos · **6 elementos** · formato: 1 slide por elemento (exceto elemento (ii), 2 slides).

---

## Slide 1 — Título e elementos do grupo

**BatEvidence — Sistema de Cadeia de Custódia Forense Digital**

- Unidade Curricular: Segurança de Sistemas de Informação (SSI)
- Curso / Turma: ___
- Data: ___

**Elementos do grupo:**

| # | Nome | Papel |
|---|---|---|
| (i) | ___ | Arquitetura, PKI e cadeias (blockchain + WORM) |
| (ii) | ___ | Requisitos de segurança e modelação de ataque |
| (iii) | ___ | Quórum Shamir, enclave e leitura condicionada |
| (iv) | ___ | Drop-box, sandbox e cenários externos (GCPD) |
| (v) | ___ | Verificação, coerência, falhas e vetores de ataque |
| (vi) | ___ | Apresentação, análise crítica e coordenação |

---

## Slide 2 — Declaração de uso de Inteligência Artificial Generativa

**Ferramentas utilizadas:** gemini
**Para quê:**

- Revisão de coerência entre notas (requisitos, fluxos, diagramas, matrizes de teste).
- Identificação de inconsistências (ex.: `v3` com três significados, `TC-RF04-02` dangling, numeração de cadeias).
- Sugestão de mais vetores de ataque adicionais (drop-box `vD1`–`vD6`, sandbox `vD7`–`vD11`).

**O que NÃO foi feito por IA:** decisões de desenho (exemplo: k=3,n=6, drop-box write-only, sandbox com três veredictos, receipt independente do veredicto) arquitetura principal, engenharia de requesitos, engenharia de segurança, validação criptográfica, e verificação final do sistema, diagramas.

**Verificação humana:** todo o output gerado foi revisto, testado contra os requisitos (RF01–RF22, RNS01–RNS24) e corrigido quando incoerente. A IA foi usada como **ferramenta de apoio**, não como autoridade.

---

## Slide 3 — Papel de cada elemento

| Elemento | Contribuição principal |
|---|---|
| (i) | Arquitetura global, PKI (Root CA / folhas), três cadeias interligadas, WORM |
| (ii) | Levantamento de requisitos (RF/RNS), modelação do sistema e de ataque |
| (iii) | Shamir k=3,n=6, enclave, leitura condicionada, pedidos efémeros |
| (iv) | Drop-box write-only, sandbox de detonação, cenários GCPD, RNS23/RNS24 |
| (v) | Verificação de coerência, falhas, vetores de ataque, matriz de testes |
| (vi) | Apresentação, análise crítica, coordenação e integração final |

---

## Slide 4 — Elemento (v): verificação, coerência e vetores de ataque

**Papel:** usar a IA e a análise manual **apenas** para verificar o sistema, a sua coerência, falhas e outros vetores de ataque. Não foi usado para desenhar — foi usado para **encontrar o que estava errado**.

**O que foi feito:**

- Auditoria de coerência completa: 11 incoerências documentadas (ex.: `v3` com três significados, referência dangling `TC-RF04-02`).
- Verificação cruzada de todos os IDs (RF01–RF22, RNS01–RNS24) contra as matrizes de teste em [[Intregavel 4]] — sem IDs órfãos, mas 29 TCs sem procedimento detalhado.
- Validação do modelo descrito pelo grupo contra as notas: 3 contradições + 5 fragilidades técnicas reportadas.

**Lição:** a IA é útil para **encontrar defeitos**, não para decidir o desenho.

---

## Slide 5 — Elemento (i): arquitetura, PKI e cadeias

- **PKI:** Alfred = Root CA (RSA-4096, HSM offline); folhas Ed25519 (32 bytes, handshakes rápidos); CRL em tempo real; RSA-PSS só na raiz e parceiros que o exijam.
- **Três cadeias interligadas:**
  1. **PKI / Trust Chain** — Alfred emite folhas; valida identidade.
  2. **Case Ledger** — sub-cadeia por caso: Bloco 0 (Genesis) → 1 (Prova) → 2 (Anotação) → 3 (Terminal).
  3. **Global Audit Sidechain** — logs de sistema (auth, quórum, wipe do enclave, acessos ao WORM).
- **WORM:** armazenamento imutável (append-only a nível de SO + Merkle + hash-chaining). Ciphertext AES-256; nunca em claro.
- **Base híbrida:** SQL (metadados, frações cifradas) + Blockchain (verdade jurídica) + Object Storage (WORM).

---

## Slide 6 — Elemento (ii): levantamento de requisitos de segurança

**46 requisitos no total: 22 RF + 24 RNS**, organizados em 4 áreas:

1. **Confidencialidade e ocultação** — RNS01 (WireGuard stealth), RNS02 (cifra em repouso com DEK), RNS03 (zero-knowledge at rest), RNS04 (zeroização).
2. **Integridade e imutabilidade** — RNS05 (WORM), RNS06 (Merkle), RNS07 (hash-chaining), RNS14 (anti-downgrade mTLS).
3. **Autenticidade e gestão de chaves** — RNS08 (assinatura Ed25519), RNS09 (recibo bilateral), RNS10 (TPM/Enclave), RNS11 (Shamir k=3,n=6), RNS13 (envelope efémero), RNS15 (HSM + 2 frações).
4. **Controlo de acesso e drop-box** — RNS12 (quarentena isolada), RNS16 (tempo fidedigno), RNS17–RNS22 (sandbox), RNS23 (isolamento do sandbox), RNS24 (anti-abuso/availability).

**RF novos (drop-box/sandbox):** RF21 (detonação), RF22 (recibo independente do veredicto), RF17 (ciclo de vida das chaves de caso).

**Matriz de rastreabilidade:** cada requisito tem linha na matriz de [[Intregavel 4]]; 29 TCs ainda sem procedimento detalhado (trabalho futuro).

---

## Slide 7 — Elemento (ii): modelação do sistema e de ataque

**Atores:** Alfred (Root CA/HSM), Batman (líder, convoca quóruns), Nightwing/Robin (terreno), Oracle (análise), GCPD (parceiro externo, drop-box).

**Cenários-chave:**
1. Recolha e depósito em campo (clonagem, hash, assinatura, recibo).
2. Abertura condicionada (quórum Shamir + HSM).
3. Análise e anotação (validação de recibos + Merkle; anotações encadeadas).
4. Submissão externa (GCPD → drop-box write-only → quarentena → triagem).

**Superfície de ataque mapeada** (em [[Ataques e Vetores]]):
- Fluxo 1: dispositivo (v1a chave privada, v1b pública, v1c certificado, v1d substituição de `PK_case`).
- Fluxo 2: envelope (v1a key substitution, v1b replay, v4/v5 downgrade, v10/v11 enclave).
- Drop-box: vD1 (flooding) … vD6 (quarentena como trampolim).
- Sandbox: vD7 (evasão) … vD11 (exfiltração via análise).

**Decisão de desenho:** vectores de ataque usam namespace `vD*` para não renumerar `v1`–`v16` (que já colidem entre Fluxo 1 e Fluxo 2 — defeito real identificado).

---

## Slide 8 — Elemento (iii): quórum Shamir, enclave e leitura condicionada

- **k=3, n=6:** 4 membros (1 fração cada) + Alfred (2 frações no HSM). Quórum = 3 membros **ou** Alfred + 1 membro. Alfred sozinho (2) < k. Consistente em todas as notas.
- **Leitura**: as frações vão **dos detentores ao enclave** (nunca ao requerente). O enclave valida-as (VSS), reconstrói `SK_case` em RAM, desenvolve a DEK, decifra, re-cifra para a chave efémera do requerente, e **zeroiza tudo** (RNS04).
- **Pedidos efémeros (RNS13):** par X25519 por pedido, nonce 128 bits, TTL curto, assinatura Ed25519 do transcript. Forward secrecy + anti-replay.
- **Modelo de chaves**: par X25519 por caso (`PK_case`/`SK_case`); DEK aleatória por prova, envolta para `PK_case`; `SK_case` nunca armazenada inteira (só frações). Rotação via bloco `ROTACAO_CHAVE`.
- **E2EE ponta-a-enclave**: o backend nunca vê o texto simples nem `SK_case`.

---

## Slide 9 — Elemento (iv): drop-box, sandbox e cenários externos

- **Drop-box (GCPD):** write-only para quarentena. Externo sem acesso à BD/WORM. Triagem pela Oracle. RNS12 (isolamento), RNS24 (anti-abuso/availability: quotas, rate-limit, recibo imediato).
- **Sandbox (RF21, RNS23):** entre o gateway e a quarentena. MicroVM com kernel próprio, imagem dourada imutável, destruída após cada amostra, egress bloqueado. Veredicto em 3 estados: `malicioso` / `limpo` / `inconclusivo` — **`inconclusivo` bloqueia admissão**.
- **Receipt independente do veredicto (RF22):** o recibo é emitido no depósito, antes do sandbox. Uma falha do sandbox não bloqueia a entrada de evidência.
- **TOCTOU (vD10):** o veredicto é indexado pelo SHA-256 dos bytes armazenados — re-hash no momento da admissão; se divergir, bloqueio.
- **Sem upload para serviços externos de análise** (RNS23): tudo corre localmente.

---

## Slide 10 — Justificação das escolhas e alterações face a sugestões de chatbots

**O que foi sugerido por chatbots e rejeitado/alterado:**

| Sugestão do chatbot | Decisão do grupo | Justificação |
|---|---|---|
| "Sistema 100% seguro" | Rejeitado | Honestidade sobre risco residual (root, destruição física, quórum malicioso) |
| Chave AES simétrica do caso dada ao depositante | Substituído por envelope assimétrico (`PK_case`/`SK_case` + DEK) | O depositante ter a chave anularia o quórum de leitura |
| CRL para revogar túneis WireGuard | Clarificado: WireGuard não tem CRL — remoção de peer via plano de gestão | CRL cobre só as folhas X.509, não peers WireGuard |
| "Validação das frações" sem mecanismo | Adicionado VSS (compromissos de Feldman) | Shamir clássico não deteta frações falsas |
| Sandbox como barreira absoluta | Tratado como análise, não garantia; 3 estados incluindo `inconclusivo` | Evasão é possível; fail-closed é a regra |
| Numeração `v1`–`v16` única | Namespace `vD*` separado | Evita colisão entre Fluxo 1 e Fluxo 2 |

**Uso da IA:** apenas para verificação (coerência, falhas, vetores) — nunca para decisões de desenho. Tudo foi revisto e corrigido pelo grupo.

---

## Slide 11 — Análise crítica

**Pontos fortes:**
- Separação clara: depósito usa `PK_case` (pública), leitura exige quórum. O servidor nunca vê o texto simples.
- Imutabilidade em camadas (WORM + Merkle + hash-chaining + audit log).
- Receipt independente do veredicto → disponibilidade da intake não depende do sandbox.
- Drop-box write-only + sandbox isolado → superfície de ataque reduzida.

**Pontos fracos / risco residual:**
- O enclave é um ponto único de confiança e de falha: se comprometido, vê todas as provas lidas.
- Alfred como CA pode forjar identidades (emissão) → não-repúdio do depósito depende da honestidade do CA. Mitigação: duplo controlo + witness externo.
- Perda de 4+ frações = caso ilegível para sempre (crypto-shredding por desenho — deve ser comunicado como tal).
- 29 TCs sem procedimento detalhado; entregáveis 1–3 e 5 vazios; erros ortográficos pendentes.
- `v3` com três significados; numeração de cadeias inconsistente em documentos antigos.

**Conclusão honesta:** o sistema é forte **onde o desenho é seguido**, mas a segurança depende da implementação correta do enclave, do HSM e do duplo controlo. Não é "100% seguro" — e não deve ser apresentado como tal.

---

## Slide 12 — Bem haja pela atenção. Perguntas?

**BatEvidence** — cadeia de custódia forense com PKI, blockchain, WORM, Shamir e E2EE ponta-a-enclave.

Obrigado pela atenção.

**Perguntas?**