---
epoch: 1795196610866
mode: agent
backendId: opencode
sessionId: "ses_e05f31472ffeGaKwXQsZQ56n8Z"
tags:
  - copilot-conversation
agentLabel: "Acesso a provas com Root CA e equipa"
usage: '{"usedTokens":74763,"contextWindow":200000,"updatedAt":1795197133574}'
---

**user**: ves se isto faz centido:
o Root CA detem 2 chaves e tem o poder de com a chave de mais um dos elementos da equipa, ver provas (ou seja e preciso um elemento da equipa mais o alfred para ver uma prova ou 3 elementos da equipa para ver uma prova) (notas incluidas
[Timestamp: 2026/11/20 17:43:30]

**ai**: Li a nota e os requisitos associados ([[Requesitos]] RNS11, [[Personas]], [[Cenarios]]). Resposta curta: **aritmeticamente faz sentido, mas como está formulado enfraquece o modelo e entra em contradição com o que o documento já afirma.** Explico porquê.

## 1. A aritmética funciona — mas obriga a mudar o $n$

Com $k=3$, se o Alfred tiver **2 frações**:

- Alfred (2) + 1 membro (1) = 3 → reconstrói ✅
- 3 membros (3) = 3 → reconstrói ✅

Só que hoje as 5 frações estão atribuídas a **4 membros + 1 do Alfred** (Bruce $x_1$, Dick $x_2$, Barbara $x_3$, Tim $x_4$, Alfred $x_5$). Se o Alfred passa a ter 2, o total passa a **6 frações** ($n=6$), ou então um dos membros perde a sua.

Isto choca diretamente com:
- RNS11: "*$k=3, n=5$*" ([[Requesitos]] linha 76);
- a tabela de hierarquia "Quórum (Shamir k=3, n=5)" (linha 83);
- [[Personas]], que atribui explicitamente 1 fração a cada um dos quatro membros.

Ou seja, não é uma alteração pontual: é mudar o requisito.

## 2. O que realmente muda no modelo de segurança

O segundo ramo ("3 elementos da equipa") **já existe** hoje: qualquer 3 dos 4 membros reconstrói. Portanto a única coisa nova é o atalho **Alfred + 1**. E esse atalho é uma redução de segurança, não uma melhoria:

| | Hoje ($k=3,n=5$) | Proposta (Alfred com 2) |
|---|---|---|
| Mínimo de pessoas | 3 independentes | **2** (Alfred + 1) |
| Compromisso do HSM | dá 1 fração → inútil | dá 2 frações → **só falta 1 membro** |
| Coerção do Alfred | não chega | **basta um membro** |
| Peso do Root | 1/5 | 2/3 do limiar |
| Regra | 3-olhos | passa a **2-pessoas** |

Para provas forenses com cadeia de custódia, baixar de "3 partes independentes" para "Root + 1" é precisamente o tipo de concentração de poder que o desenho atual evita. O Alfred passa de **custódio neutro** a **quase-master**: quem tiver o HSM e convencer/comprometer **um** membro abre tudo.

## 3. Contradiz o que a nota diz sobre o Alfred

A nota é explícita em vários pontos:

- linha 85: "*Não deposita provas nem participa como votante*";
- linha 594: "*detida em escrow pelo Root CA (Alfred, HSM offline), que não vota nem deposita provas — apenas custódia*";
- o diagrama de casos de uso liga o Alfred só a `UC_HSM` e `UC_PKI`, **não** a `UC_SubmitKey`.

Com 2 frações, o Alfred tem de **submeter frações** para reconstruir → passa a ser votante de facto, e o diagrama de casos de uso fica errado (falta `AL --- UC_SubmitKey`).

## 4. "Notas incluídas" — isso já está correto

As notas são cifradas com a **mesma chave AES do caso** que a evidência (linhas 425 e 249). Logo, o mesmo quórum que abre a prova abre as notas. Não é preciso acrescentar nada — já é assim. A única nuance é que as notas **privadas** são filtradas antes de chegarem ao cliente (linha 275), mas a cifra é a mesma.

## 5. Se quiseres mesmo adotar isto

É defensável **se for uma decisão consciente** e documentada como *threshold ponderado* (Alfred peso 2, membros peso 1, limiar 3, $n=6$). Nesse caso tens de atualizar, no mínimo:

1. **RNS11** → $k=3, n=6$ (ou explicitar a redistribuição);
2. tabela de hierarquia e [[Personas]] (Alfred com $x_5, x_6$; membros com 1 cada);
3. **Notas de Coerência** (linha 594) — remover "não vota";
4. diagrama de casos de uso — `AL --- UC_SubmitKey`;
5. [[Cenarios]] — a lista "$x_1 \ldots x_4$" passa a incluir as frações do Alfred;
6. **modelo de ameaças** — acrescentar explicitamente: "compromisso do HSM + 1 membro = quebra total".

E, como compensação, era bom exigir controlos adicionais no HSM (duplo controlo físico, autorização assinada do caso antes de libertar as frações, auditoria obrigatória do `QUORUM_INIT`).

## 6. Alternativa que preserva o espírito

Se o objetivo operacional é "*o Alfred está sempre presente na abertura*" (é o que [[Cenarios]] descreve: ele reúne as frações no HSM), consegues isso **sem lhe dar peso criptográfico**:

- Mantém $k=3, n=5$ (1 fração cada).
- Torna a presença do Alfred um **requisito de cerimónia**: o HSM só inicia a sessão com a chave física/PIN dele, e o `QUORUM_INIT` tem de ser coassinado por ele — mas as frações continuam a valer 1 cada.
- Assim continuas a precisar de **3 membros** para o segredo, e o Alfred autoriza/opera o HSM sem se tornar o elo que reduz o limiar a 2.

**Veredicto:** a ideia não está errada matematicamente, mas tal como a descreves **baixa o quórum efetivo para 2 pessoas e contradiz a documentação atual do Alfred**. Se for para avançar, fá-lo como *threshold ponderado* explícito, com $n=6$, modelo de ameaças revisto e as secções acima corrigidas. Queres que eu aplique essa versão na nota (ou, em alternativa, a versão "cerimónia" que mantém $k=3,n=5$)?
[Timestamp: 2026/11/20 17:43:30]

**user**: vamos implementar essa correcao para o alfred para o shamir pode contribuir com duas chaves
[Timestamp: 2026/11/20 17:47:25]

**ai**: Vou implementar a correção em todos os sítios onde o esquema Shamir é definido. Primeiro leio os ficheiros que precisam de ser coerentes.Agora aplico as alterações. Começo pela nota principal.Agora os requisitos, personas, cenários e vetores de ataque.Verifico se ficaram referências antigas ao esquema $n=5$ em SSI/batevidence.Implementado. O esquema passou de **$k=3, n=5$** para **$k=3, n=6$**, com o Alfred a deter **2 frações** ($x_5$, $x_6$) e cada membro 1. O limiar de 3 passa a ser atingível por **Alfred + 1 membro** ou por **3 membros sem o Alfred**.

## Ficheiros alterados

**[[Bat (couf couf) Evidence A Wayne]]**
- Tabela de hierarquia: coluna de quórum → `k=3, n=6`; linha do Root passa a "Detém 2 frações ($x_5$, $x_6$); inicia a sessão no HSM" e o âmbito deixa de dizer "não participa como votante".
- Intro da hierarquia e **Regras transversais** (quórum e segregação de funções) atualizadas com as duas vias de acesso.
- Diagrama de casos de uso: adicionado `AL --- UC_SubmitKey` (o Alfred passa a submeter frações).
- Base de dados: nota de que as 2 frações do Alfred residem no **HSM**, não na `T_SHAMIR`.
- **Notas de Coerência**: substituída a antiga "5.ª fração em escrow" pela regra $k=3, n=6$, incluindo explicitamente o risco "compromisso do HSM + 1 membro".

**[[Requesitos]]**
- RNS03: `n=5` → `n=6`.
- RNS11: título e texto passam a `k=3, n=6`, com a distribuição (4 membros × 1 + Alfred × 2) e a garantia de que o Alfred sozinho fica abaixo do limiar.

**[[Personas]]**
- Alfred: passa a "Guardião de Custódia… detém 2 frações ($x_5$, $x_6$)", podendo completar o limiar com 1 membro, mas não sozinho.

**[[Cenarios]]**
- Cenário 2: narrativa, objetivo e "o que está envolvido" refletem as duas vias (Alfred + 1, ou 3 membros) e as frações $x_5/x_6$.

**[[Ataques e Vetores]]**
- v7: mitigação passa a "k=3: 2 do Alfred + 1 membro, ou 3 membros".
- v9: validação de que as frações do Alfred só são libertadas pelo HSM.
- Novo vetor **v9a**: "Compromisso do HSM + 1 membro reduz o limiar efetivo a 2 pessoas", com mitigações (duplo controlo físico, ordem de caso assinada, auditoria do `QUORUM_INIT`).

## Nota sobre a matemática

Isto é, na prática, um **threshold ponderado** (Alfred peso 2, restantes peso 1, limiar 3). Continua a garantir segurança teórica da informação — 2 frações não dão vantagem — mas baixa o mínimo de pessoas de 3 para 2 no caminho Alfred + 1. Ficou documentado como decisão consciente, não como detalhe implícito.

Se quiseres, posso também ajustar a [[Apresentação]] ou os diagramas de sequência para mencionarem explicitamente as duas vias de quórum.
[Timestamp: 2026/11/20 17:47:25]

**user**: coloca todos os requesitos que possam faltar daqui
[Timestamp: 2026/11/20 17:50:34]

**ai**: Vou comparar a nota principal com o ficheiro de requisitos para identificar o que está em falta.Vou acrescentar os requisitos que a nota principal já descreve mas que não estão formalizados em [[Requesitos]], e corrigir a referência `AGENT-00`.A ordem das secções ficou trocada (2.5 antes de 2.4). Vou corrigir.Adicionei a [[Requesitos]] os requisitos que a nota principal já descreve mas que não estavam formalizados, e corrigi uma referência errada. Ficaram **10 RF novos** (RF11–RF20) e **9 RNS novos** (RNS14–RNS22).

## Requisitos Funcionais acrescentados

| ID | Requisito | Onde estava na nota |
|---|---|---|
| **RF11** | Autenticação mútua (X.509/Ed25519 + mTLS/WireGuard) e autorização por caso | "Hierarquia de acesso", "Acesso Seguro" |
| **RF12** | Registo encadeado do caso (Genesis → prova/nota → terminal, com hash e assinatura) | "Esquema de Blockchain" |
| **RF13** | Encerramento e selagem criptográfica (bloco terminal + smart contract LOCK) | Fase 4 do ciclo de vida |
| **RF14** | Cadeia de Auditoria Global (acessos, quórum, `ENCLAVE_WIPE`, PKI, WORM) | "Cadeia de Auditoria Global" |
| **RF15** | Consulta com quórum e entrega E2EE (re-cifragem no enclave) | "Acesso Seguro a Evidências" |
| **RF16** | Separação de notas públicas vs. privadas e filtragem no backend | Passo 2 e 4 do fluxo de consulta |
| **RF17** | Ciclo de vida da chave de caso (gerar AES, dividir, distribuir, destruir) | Fase 1 do depósito |
| **RF18** | Verificação contínua de revogação (CRL) em todas as operações | PKI / CRL |
| **RF19** | Provas em ficheiro **e** em registo estruturado | Enunciado ("ficheiros ou registos estruturados") |
| **RF20** | Recibo de depósito com prova de inclusão Merkle | Recibo + árvore de Merkle |

## Requisitos Não-Funcionais acrescentados

Criei a secção **2.5. Perímetro, Hardware e Operação**:

- **RNS14** — mTLS interno e prevenção de downgrade (lista de algoritmos permitidos).
- **RNS15** — Duplo controlo no HSM para as 2 frações do Alfred (derivado da alteração de quórum).
- **RNS16** — Carimbo temporal fidedigno e sincronização de relógio (±60 s).
- **RNS17** — Redundância e recuperação (principal, hot backup, cold backup).
- **RNS18** — Segregação de redes e firewalls físicas.
- **RNS19** — Menor privilégio e segregação de funções.
- **RNS20** — Auditoria assinada de todas as operações C/R/D.
- **RNS21** — Isolamento do Secure Enclave.
- **RNS22** — Desempenho em liga satélite (MTU/MSS/BBR/keepalive).

## Correção

- **RF10**: `AGENT-00` → **Root CA (Alfred, HUM-ALFRED-00)**, e acrescentado "publicar a CRL".

Também reorganizei as secções para a ordem correta (2.4 antes de 2.5), que tinha ficado trocada na inserção.

Se quiseres, o passo seguinte natural é mapear cada RF/RNS novo para os **testes do [[Intregavel 4]]** e para os vetores de ataque, mas isso já sai do âmbito de "requisitos".
[Timestamp: 2026/11/20 17:50:34]