# Definição de Testes de Conformidade

> Definição dos testes necessários para validar a conformidade com os requisitos estabelecidos, com ênfase particular nos requisitos de segurança.

## 1. Objetivo e âmbito

Este documento define a estratégia, os casos de teste e os critérios de aceitação para validar o **Bat(couf couf)Evidence** contra os requisitos de [[Requesitos]] (RF01–RF22, RNS01–RNS24) e contra os vetores de ataque identificados em [[Ataques e Vetores]].

O âmbito cobre:
- o ciclo de vida do caso e da prova (abertura, depósito, anotação, encerramento);
- a PKI, a autenticação e o controlo de acesso;
- a integridade WORM, a árvore de Merkle e o ledger do caso;
- o quórum Shamir ($k=3, n=6$, com 2 frações no Root CA);
- a entrega E2EE e o Secure Enclave;
- a cadeia de auditoria global;
- a operação em rede satélite e a recuperação de desastres.

As explicações de cada tipo de ataque — escritas para quem não conhece o conceito — estão reunidas em [[Tipos de Ataque]], na pasta `Explicacoes e material/ataques`.

## 2. Estratégia de teste

| Nível                       | Objetivo                                 | Exemplos                                                         |
| --------------------------- | ---------------------------------------- | ---------------------------------------------------------------- |
| **Unitário**                | Validar primitivas e funções isoladas    | Shamir, X25519→HKDF→ChaCha20-Poly1305, Ed25519, hash-chain       |
| **Integração**              | Validar a interação entre serviços       | PKI ↔ CRL, backend ↔ WORM, quórum ↔ enclave                      |
| **Sistema**                 | Validar fluxos completos ponta a ponta   | depósito no terreno, consulta com quórum, encerramento de caso   |
| **Segurança / adversarial** | Tentar quebrar as garantias              | replay, downgrade, IDOR, injeção de frações, adulteração de logs |
| **Aceitação**               | Confirmar conformidade com os requisitos | matriz de rastreabilidade e evidências                           |

**Princípio:** cada requisito tem pelo menos um teste positivo (comportamento esperado) e, quando aplicável, um teste negativo (tentativa de violação que deve ser rejeitada). Os testes de segurança são **fail-closed**: qualquer ambiguidade conta como falha.

## 3. Ambiente e ferramentas

| Componente | Configuração de teste |
| --- | --- |
| PKI | Root CA RSA-4096 em SoftHSM/HSM de laboratório; folhas Ed25519; CRL versionada |
| Transporte | WireGuard (exterior) e mTLS (interior); `tc netem` para simular latência GEO/LEO |
| Armazenamento | S3 com Object Lock (WORM) ou NAS com permissões append-only |
| Enclave | SGX/SEV ou enclave simulado com isolamento de processo e *zeroing* verificável |
| Criptografia | OpenSSL 3.x, libsodium, biblioteca Shamir auditada |
| Análise de tráfego | Wireshark, tcpdump, mitmproxy |
| Testes | pytest/Playwright para fluxos; scripts de adversário para casos negativos |
| Observabilidade | Logs da sidechain de auditoria, métricas de latência e de falhas |

## 4. Critérios de entrada, saída e classificação

- **Entrada:** requisitos congelados, ambiente disponível, dados de teste e certificados emitidos.
- **Saída:** todos os testes `PASS`, ou desvios registados com risco aceite formalmente.
- **Classificação:** `PASS`, `FAIL`, `BLOCKED`, `NOT APPLICABLE`.
- **Evidência:** captura de logs, hashes, pacotes, relatório assinado pelo executor.
- **Severidade de falha:** Crítica (quebra de confidencialidade/integridade), Alta (bypass de controlo), Média (degradação), Baixa (usabilidade).

---

## 5. Matriz de rastreabilidade — Requisitos Funcionais

| Requisito                               | Teste(s)                   | Nível                | Vetor(es)           | Critério de aceitação                                     |
| --------------------------------------- | -------------------------- | -------------------- | ------------------- | --------------------------------------------------------- |
| RF01 Criação/gestão de casos            | TC-RF01-01, TC-RF01-02     | Sistema              | v8, v9              | Caso criado com Genesis; casos isolados entre si          |
| RF02 Ingestão no terreno                | TC-RF02-01..03             | Sistema              | v1, v1a             | Prova depositada, assinada e atribuída ao autor           |
| RF03 Cross-referencing                  | TC-RF03-01                 | Integração           | —                   | Mesmo URI em 2 casos, sem duplicação física               |
| RF04 Drop-box público                   | TC-RF04-01                 | Sistema              | v3                  | Submissão write-only; leitura negada                      |
| RF05 Admissão da quarentena             | TC-RF05-01                 | Sistema              | v3, v8              | Só Oracle admite; original preservado                     |
| RF06 Anotações encadeadas               | TC-RF06-01, TC-RF06-02     | Sistema              | v6                  | Nota encadeada; prova original inalterada                 |
| RF07 Auditoria de integridade           | TC-RF07-01                 | Sistema              | v6a, v11            | Hash recalculado coincide com Merkle/assinatura           |
| RF08 Convocação de quórum               | TC-RF08-01                 | Sistema              | v7, v8              | Sessão registada; sem quórum não há decifra               |
| RF09 Reconstituição em RAM              | TC-RF09-01                 | Integração           | v7, v9              | Chave reconstruída só em RAM volátil                      |
| RF10 Ciclo de vida de certificados      | TC-RF10-01, TC-RF10-02     | Integração           | v1c, v13            | Emissão/revogação refletida na CRL                        |
| **RF11 Autenticação e autorização**     | **TC-RF11-01..03**         | **Sistema**          | **v1b, v4, v8, v9** | **Sem certificado válido ou fora do caso → negação**      |
| **RF12 Ledger do caso**                 | **TC-RF12-01, TC-RF12-02** | **Integração**       | **v6, v6a**         | **Alterar um bloco invalida os seguintes**                |
| **RF13 Encerramento e LOCK**            | **TC-RF13-01, TC-RF13-02** | **Sistema**          | **v8**              | **Após LOCK, novos depósitos/notas rejeitados**           |
| **RF14 Auditoria global**               | **TC-RF14-01, TC-RF14-02** | **Integração**       | **v6a, v12**        | **Evento registado sem segredos nem chaves**              |
| **RF15 Consulta E2EE**                  | **TC-RF15-01, TC-RF15-02** | **Sistema**          | **v12, v14**        | **Backend não consegue ler o payload**                    |
| **RF16 Notas públicas/privadas**        | **TC-RF16-01, TC-RF16-02** | **Sistema**          | **v6, v14**         | **Notas privadas ausentes na entrega a terceiros**        |
| **RF17 Chaves de caso**                 | **TC-RF17-01..03** | **Integração**       | **v11, v1d**        | **`SK_case` só existe em frações; só quórum reconstrói**   |
| **RF18 CRL contínua**                   | **TC-RF18-01, TC-RF18-02** | **Integração**       | **v1c, v7**         | **Certificado revogado/expirado rejeitado**               |
| **RF19 Ficheiro e registo estruturado** | **TC-RF19-01, TC-RF19-02** | **Unitário/Sistema** | **v1**              | **Hash canónico estável e verificável**                   |
| **RF20 Recibo e prova Merkle**          | **TC-RF20-01, TC-RF20-02** | **Integração**       | **v14, v15**        | **Prova de inclusão válida sem revelar conteúdo**         |
| RF21 Sandbox de detonação               | TC-RF21-01..03             | Integração           | vD2, vD7, vD10      | Veredicto ligado ao hash; evasão e cifrados não admitidos |
| RF22 Recibo independente do veredicto   | TC-RF22-01                 | Sistema              | vD9                 | Sandbox indisponível → recibo emitido, admissão pendente  |

## 6. Matriz de rastreabilidade — Requisitos Não-Funcionais e de Segurança

| Requisito | Teste(s) | Nível | Vetor(es) | Critério de aceitação |
| --- | --- | --- | --- | --- |
| RNS01 Stealth VPN | TC-RNS01-01 | Sistema | v2 | Só WireGuard; portas fechadas a pacotes não autenticados |
| RNS02 Cifra em repouso | TC-RNS02-01 | Integração | v11 | Sem AES-256-GCM não há leitura do objeto |
| RNS03 Destruição da `SK_case` | TC-RNS03-01 | Integração | v11 | `SK_case` ausente de disco/RAM após a leitura |
| RNS04 Zeroização | TC-RNS04-01 | Sistema | v13, v16 | RAM com zeros; sem resíduos em crash dump |
| RNS05 WORM | TC-RNS05-01 | Sistema | v6a, v11 | `UPDATE`/`DELETE` negados após fecho |
| RNS06 Merkle | TC-RNS06-01 | Integração | v6a | Raiz recalculada; prova de inclusão válida |
| RNS07 Hash-chain de notas | TC-RNS07-01 | Integração | v6 | Remover/alterar nota quebra a cadeia |
| RNS08 Assinaturas | TC-RNS08-01, TC-RNS08-02 | Unitário | v10 | Ed25519 em provas/notas; RSA só onde permitido |
| RNS09 Recibo bilateral | TC-RNS09-01 | Sistema | v14 | Recibo assinado com timestamp verificável |
| RNS10 TPM/Enclave | TC-RNS10-01 | Sistema | v1a, v16 | Chave privada não exportável |
| RNS11 Shamir $k=3,n=6$ | TC-RNS11-01..04 | Integração | v7, v9 | 3 frações válidas reconstroem; <3 não revelam nada; fração corrompida detetada pelo compromisso VSS |
| RNS12 Quarentena | TC-RNS12-01 | Sistema | v3 | Externo sem acesso à BD/WORM |
| RNS13 Envelope efémero | TC-RNS13-01..03 | Sistema | v1, v1a, v1b | Replay/TTL/key substitution rejeitados |
| **RNS14 mTLS e anti-downgrade** | **TC-RNS14-01, TC-RNS14-02** | **Integração** | **v5, v7** | **Negociação para algoritmo fraco rejeitada** |
| **RNS15 HSM e 2 frações** | **TC-RNS15-01..03** | **Sistema** | **v9, v13** | **Libertação exige duplo controlo e fica auditada** |
| **RNS16 Tempo fidedigno** | **TC-RNS16-01, TC-RNS16-02** | **Sistema** | **v1b** | **Relógio fora da janela → rejeição** |
| **RNS17 Redundância** | **TC-RNS17-01, TC-RNS17-02** | **Sistema** | — | **Failover e restauro preservam hashes** |
| **RNS18 Firewalls** | **TC-RNS18-01** | **Sistema** | **v3, v4** | **Segmentação impede movimento lateral** |
| **RNS19 Menor privilégio** | **TC-RNS19-01, TC-RNS19-02** | **Sistema** | **v4, v8** | **Cada papel só faz o que lhe compete** |
| **RNS20 Auditoria C/R/D** | **TC-RNS20-01** | **Integração** | **v6a** | **Toda a operação gera evento assinado** |
| **RNS21 Isolamento do enclave** | **TC-RNS21-01** | **Sistema** | **v10, v11** | **Enclave não expõe chaves nem recebe dados a mais** |
| **RNS22 Liga satélite** | **TC-RNS22-01** | **Sistema** | **v2** | **Throughput/latência dentro do orçamento definido** |
| **RNS23 Isolamento da sandbox** | **TC-RNS23-01, TC-RNS23-02** | **Sistema** | **vD8, vD11** | **Sem escape, sem egress e sem credenciais dentro da VM** |
| **RNS24 Anti-abuso da drop-box** | **TC-RNS24-01** | **Sistema** | **vD1, vD9** | **Limites aplicados; recibo emitido sob flooding** |

---

## 7. Casos de teste detalhados — novos requisitos funcionais

### TC-RF11-01 — Autenticação mútua obrigatória
- **Objetivo:** garantir que nenhuma operação ocorre sem certificado de folha válido.
- **Procedimento:** tentar aceder ao backend (a) sem certificado, (b) com certificado autoassinado, (c) com certificado de outra CA.
- **Esperado:** `403`/handshake falhado em (a)-(c); acesso permitido apenas com cadeia válida até à Root CA e estado `good` na CRL.
- **Vetores:** v1b, v4.

### TC-RF11-02 — Autorização por caso (anti-IDOR)
- **Objetivo:** validar que um membro só acede aos casos que lhe estão atribuídos.
- **Procedimento:** autenticado como Nightwing, pedir um caso não atribuído iterando IDs.
- **Esperado:** negação consistente, sem revelar existência do caso; evento na auditoria.
- **Vetores:** v8, v9.

### TC-RF11-03 — Default deny
- **Objetivo:** confirmar negação por omissão para papéis não previstos.
- **Procedimento:** criar identidade sem papel e tentar `C/R/D` sobre caso e WORM.
- **Esperado:** todas as operações negadas; nenhum efeito colateral.

### TC-RF12-01 — Integridade da cadeia de blocos
- **Objetivo:** validar o encadeamento `Prev_Hash`.
- **Procedimento:** criar Genesis, prova e nota; verificar hashes; alterar o payload do bloco 1 na BD.
- **Esperado:** a verificação falha a partir do bloco 1 e todos os seguintes.
- **Vetores:** v6, v6a.

### TC-RF12-02 — Inserção/remoção retroativa
- **Objetivo:** impedir reordenação ou eliminação de blocos.
- **Procedimento:** tentar inserir um bloco entre dois existentes e apagar um bloco intermédio.
- **Esperado:** rejeição pelo smart contract/ledger; auditoria do tentante.

### TC-RF13-01 — Bloco terminal e LOCK
- **Objetivo:** validar o encerramento formal.
- **Procedimento:** encerrar caso; verificar bloco terminal com flag `Encerrado` e assinatura.
- **Esperado:** bloco gravado; estado do caso passa a `Fechado`.
- **Vetores:** v8.

### TC-RF13-02 — Rejeição pós-encerramento
- **Objetivo:** garantir imutabilidade após o LOCK.
- **Procedimento:** após LOCK, tentar depositar prova e adicionar nota.
- **Esperado:** ambas rejeitadas; nenhuma alteração no WORM nem no ledger.

### TC-RF14-01 — Cobertura de eventos
- **Objetivo:** garantir que os eventos de sistema são registados.
- **Procedimento:** executar login mTLS, quórum, `ENCLAVE_WIPE`, emissão/revogação e acesso ao WORM.
- **Esperado:** um bloco assinado por evento, com autor, timestamp e resultado.

### TC-RF14-02 — Ausência de segredos na auditoria
- **Objetivo:** impedir fuga por logs.
- **Procedimento:** inspecionar todos os campos da sidechain após um quórum completo.
- **Esperado:** apenas `request_id`, hash do nonce e metadados; nunca chaves, frações ou segredo ECDH.
- **Vetores:** v6a, v12.

### TC-RF15-01 — E2EE contra o backend
- **Objetivo:** validar a re-cifragem para a chave efémera.
- **Procedimento:** instrumentar o backend para tentar decifrar o payload devolvido pelo enclave.
- **Esperado:** o backend obtém apenas bytes ilegíveis; o cliente decifra com a chave efémera.
- **Vetores:** v12.

### TC-RF15-02 — Chave efémera de uso único
- **Objetivo:** impedir reutilização de envelope.
- **Procedimento:** reenviar o mesmo `request_id`/nonce após uma resposta.
- **Esperado:** rejeição; `request_id` marcado como consumido.
- **Vetores:** v1b.

### TC-RF16-01 — Filtragem de notas privadas
- **Objetivo:** impedir que notas privadas cheguem a terceiros.
- **Procedimento:** caso com nota pública e nota privada; consultar como outro membro.
- **Esperado:** apenas a pública é entregue; a privada não aparece nem em claro nem cifrada para o requerente.
- **Vetores:** v6, v14.

### TC-RF16-02 — Integridade das notas após filtragem
- **Objetivo:** garantir que filtrar não quebra a cadeia.
- **Procedimento:** recalcular a cadeia de notas públicas após remoção das privadas.
- **Esperado:** a prova de integridade continua válida para o subconjunto público.

### TC-RF17-01 — Ciclo de vida da chave de caso
- **Objetivo:** validar geração no enclave, divisão e distribuição; confirmar que a pública está comprometida no Genesis.
- **Procedimento:** abrir caso, inspecionar os metadados e a `T_SHAMIR`, o HSM e o Bloco 0; comparar `PK_case` nos três locais.
- **Esperado:** 6 frações (4 membros + 2 no HSM) cifradas para o detentor, com compromissos VSS publicados; `SK_case` ausente; `SHA-256(PK_case ‖ case_id)` igual no Genesis e nos metadados.
- **Vetores:** v11, v1d.

### TC-RF17-02 — Inexistência da chave privada em repouso
- **Objetivo:** confirmar *zero-knowledge at rest*.
- **Procedimento:** procurar `SK_case` e DEKs em claro em disco, swap, *core dumps*, logs e cache do enclave após distribuição e após uma leitura completa.
- **Esperado:** nenhuma cópia recuperável; reconstrução só por quórum, em RAM, com zeroização no fim.
- **Vetores:** v11.

### TC-RF17-03 — Substituição de `PK_case`
- **Objetivo:** validar que uma troca da chave pública do caso é detetada antes do depósito.
- **Procedimento:** alterar `PK_case` nos metadados e tentar depositar; o cliente deve comparar com o compromisso do Genesis.
- **Esperado:** depósito recusado; divergência registada na cadeia de auditoria.
- **Vetores:** v1d.

### TC-RF18-01 — Certificado revogado
- **Objetivo:** validar a CRL em tempo real.
- **Procedimento:** revogar o certificado de Nightwing e tentar autenticar/assinar.
- **Esperado:** rejeição imediata em todas as operações.
- **Vetores:** v1c, v7.

### TC-RF18-02 — Certificado expirado / fora da janela
- **Objetivo:** validar validade temporal.
- **Procedimento:** usar certificado expirado e relógio desviado.
- **Esperado:** rejeição *fail-closed*.
- **Vetores:** v1b.

### TC-RF19-01 — Hash de registo estruturado
- **Objetivo:** garantir representação canónica.
- **Procedimento:** serializar o mesmo registo com ordens de campos diferentes e comparar hashes.
- **Esperado:** hash idêntico após canonicalização; divergência detetada se o conteúdo mudar.

### TC-RF19-02 — Preservação do formato original
- **Objetivo:** garantir verificação futura.
- **Procedimento:** depositar JSON/XML e recuperar após 30 dias simulados.
- **Esperado:** bytes originais preservados e hash reconferido com sucesso.

### TC-RF20-01 — Recibo assinado
- **Objetivo:** validar o comprovativo de depósito.
- **Procedimento:** depositar prova e verificar assinatura, hash e timestamp do recibo.
- **Esperado:** recibo válido e verificável por terceiro.
- **Vetores:** v14, v15.

### TC-RF20-02 — Prova de inclusão Merkle
- **Objetivo:** permitir verificação sem revelar conteúdo.
- **Procedimento:** gerar prova de inclusão para uma folha e verificar contra a raiz publicada.
- **Esperado:** prova válida; folha alterada invalida a prova.
- **Vetores:** v6a.

### TC-RF21-01 — Veredicto ligado ao objeto armazenado
- **Objetivo:** impedir TOCTOU entre análise e entrega.
- **Procedimento:** submeter a amostra A; durante a detonação, tentar substituir o objeto no buffer e reenviar B com o mesmo nome; abrir depois como Oracle.
- **Esperado:** o re-hash na abertura coincide com o `SHA-256` do veredicto; qualquer divergência bloqueia a admissão e gera alarme na auditoria.
- **Vetores:** vD10.

### TC-RF21-02 — Evasão *sandbox-aware*
- **Objetivo:** garantir que a evasão produz `inconclusivo`, nunca `limpo`.
- **Procedimento:** amostra de teste que dorme 10 min antes de agir e deteta artefactos de VM; correr com a janela padrão (180 s) e depois com *jitter*/janela alargada.
- **Esperado:** na configuração endurecida o resultado é `inconclusivo`, a admissão fica bloqueada e o relatório regista o motivo; nenhum caso devolve `limpo`.
- **Vetores:** vD7.

### TC-RF21-03 — Arquivo cifrado e *zip bomb*
- **Objetivo:** validar o tratamento de conteúdo opaco e de bombas de descompressão.
- **Procedimento:** submeter ZIP com palavra-passe e ZIP com rácio de compressão acima do limite.
- **Esperado:** o cifrado fica `inconclusivo` com pedido de palavra-passe à Oracle; a bomba é rejeitada no pré-filtro, sem detonação, com evento auditado.
- **Vetores:** vD7, vD9, vD10.

### TC-RF22-01 — Sandbox indisponível
- **Objetivo:** garantir disponibilidade do depósito sem perder custódia.
- **Procedimento:** desligar o serviço de sandbox e efetuar um depósito válido.
- **Esperado:** recibo assinado emitido com hash e timestamp; depósito em `pendente_analise`; admissão negada até existir veredicto; na recuperação a fila reprocessa sem duplicar objetos.
- **Vetores:** vD9.

---

## 8. Casos de teste detalhados — novos requisitos de segurança

### TC-RNS14-01 — mTLS entre serviços internos
- **Procedimento:** tentar comunicação serviço-a-serviço sem mTLS e com certificado de serviço não autorizado.
- **Esperado:** handshake rejeitado; apenas serviços com certificado válido comunicam.
- **Vetores:** v4, v5.

### TC-RNS14-02 — Prevenção de downgrade
- **Procedimento:** forçar a negociação para SHA-1/RSA-1024/Ed25519 fora da lista.
- **Esperado:** rejeição; nenhum fallback silencioso.
- **Vetores:** v5, v7.

### TC-RNS15-01 — Duplo controlo no HSM
- **Procedimento:** tentar libertar as 2 frações do Alfred sem presença física e sem autorização assinada.
- **Esperado:** libertação negada; tentativa registada na auditoria.
- **Vetores:** v9, v13.

### TC-RNS15-02 — Auditoria da libertação
- **Procedimento:** libertar frações com autorização válida.
- **Esperado:** evento `QUORUM_INIT` com caso, autor, hora e resultado; sem conteúdo das frações.

### TC-RNS15-03 — Deteção de compromisso HSM + 1 membro
- **Procedimento:** simular HSM comprometido e uma fração de membro; verificar alertas.
- **Esperado:** a combinação é detetável por anomalia de padrão/auditoria; alarme gerado.
- **Observação:** este teste documenta o risco residual aceite do modelo $k=3,n=6$.

### TC-RNS16-01 — Janela de relógio
- **Procedimento:** enviar envelope com timestamp fora de ±60 s.
- **Esperado:** rejeição.
- **Vetores:** v1b.

### TC-RNS16-02 — Timestamp de blocos e recibos
- **Procedimento:** comparar timestamps com a fonte fidedigna.
- **Esperado:** monotonia e coerência; desvios sinalizados.

### TC-RNS17-01 — Failover
- **Procedimento:** desligar o servidor principal durante um depósito.
- **Esperado:** hot backup assume; nenhuma prova perdida ou duplicada.

### TC-RNS17-02 — Restauro de backup
- **Procedimento:** restaurar cold backup e verificar hashes/Merkle.
- **Esperado:** cadeia de custódia intacta; divergências detetadas.

### TC-RNS18-01 — Segmentação de rede
- **Procedimento:** a partir da drop-box externa, tentar alcançar BD, WORM e serviços internos.
- **Esperado:** todo o tráfego bloqueado; apenas o gateway de quarentena é alcançável.
- **Vetores:** v3, v4.

### TC-RNS19-01 — Menor privilégio
- **Procedimento:** para cada papel, tentar operações fora do seu âmbito.
- **Esperado:** negação; matriz de permissões coincide com a tabela de hierarquia.

### TC-RNS19-02 — Segregação de funções
- **Procedimento:** tentar que o depositante valide/anote e que o Alfred deposite.
- **Esperado:** operações rejeitadas; auditoria regista a tentativa.

### TC-RNS20-01 — Auditoria C/R/D
- **Procedimento:** executar criação, leitura e revogação.
- **Esperado:** três eventos assinados, encadeados e atribuídos ao autor.

### TC-RNS21-01 — Isolamento do enclave
- **Procedimento:** inspecionar chamadas e memória do enclave; tentar ler chaves do exterior.
- **Esperado:** chaves inacessíveis; RAM limpa após a operação; apenas dados mínimos recebidos.
- **Vetores:** v10, v11.

### TC-RNS22-01 — Desempenho satélite
- **Procedimento:** medir throughput/latência com `tc netem` em perfis LEO (30–70 ms) e GEO (600–800 ms), com MTU/MSS/BBR configurados.
- **Esperado:** dentro do orçamento definido; sem _stalls_ por fragmentação; keepalive estável.
- **Vetores:** v2.

### TC-RNS23-01 — Isolamento e escape da sandbox
- **Procedimento:** dentro da VM, tentar montar o *host*, alcançar BD/WORM/backend, ler credenciais e explorar o kernel; inspecionar o estado entre amostras.
- **Esperado:** nenhuma montagem do *host*; tráfego interno bloqueado; sem credenciais dentro da VM; tentativa de escape detetada, VM destruída e amostra seguinte arranca de imagem limpa.
- **Vetores:** vD8.

### TC-RNS23-02 — *Egress* da VLAN de análise
- **Procedimento:** amostra tenta DNS/HTTP/ICMP para a Internet e para destinos internos.
- **Esperado:** apenas o *sinkhole* responde; todo o resto bloqueado e registado; nenhum pacote sai da VLAN.
- **Vetores:** vD8, vD11.

### TC-RNS24-01 — Quota, fila e *flooding*
- **Procedimento:** exceder quota e limite de tamanho; enviar 1000 depósitos em 1 minuto; medir fila e latência da triagem.
- **Esperado:** excedentes recusados *fail-closed* e auditados; depósitos dentro da quota recebem recibo; a fila não excede a profundidade definida e a triagem degrada de forma controlada.
- **Vetores:** vD1, vD9.

---

## 9. Testes específicos do quórum Shamir ($k=3, n=6$)

| ID | Cenário | Entrada | Resultado esperado |
| --- | --- | --- | --- |
| **TC-Q-01** | Alfred + 1 membro | 2 frações do HSM + 1 fração de membro | Reconstrução bem-sucedida |
| **TC-Q-02** | Alfred + qualquer membro | 2 + 1, variando o membro | Sucesso em todos os casos |
| **TC-Q-03** | 3 membros sem Alfred | 3 frações de membros | Reconstrução bem-sucedida |
| **TC-Q-04** | Alfred sozinho | 2 frações | Falha; segredo não revelado |
| **TC-Q-05** | 2 membros | 2 frações | Falha; nenhuma vantagem information-theoretic |
| **TC-Q-06** | 2 membros + 1 fração inválida | 2 válidas + 1 corrompida | Falha; validação rejeita; auditoria regista |
| **TC-Q-07** | Fração do Alfred sem duplo controlo | 2 frações sem autorização | Libertação negada (RNS15) |
| **TC-Q-08** | HSM indisponível | 3 membros | Sucesso pela via alternativa; disponibilidade preservada |
| **TC-Q-09** | Replay de submissão de frações | Reenvio da mesma sessão | Rejeição por `request_id`/nonce |
| **TC-Q-10** | Rotação/revogação de fração | Substituir fração de um membro | Nova fração válida; antiga rejeitada |
| **TC-Q-11** | Auditoria do quórum | Sessão completa | Evento sem frações nem segredo; apenas metadados |
| **TC-Q-12** | Análise estatística com <3 frações | 0, 1 e 2 frações | Nenhuma correlação com o segredo (teste de não-vantagem) |

**Critério transversal:** qualquer caminho que produza o segredo com menos de 3 frações é `FAIL` crítico, exceto se for um canal de recuperação explicitamente previsto e auditado.

---

## 10. Testes adversariais e mapeamento aos vetores

| Vetor   | Teste                      | O que se tenta                                           | Resultado esperado                     | Ataque                                                                         |
| ------- | -------------------------- | -------------------------------------------------------- | -------------------------------------- | ------------------------------------------------------------------------------ |
| v1/v1a  | TC-RNS10-01                | Extrair chave privada do dispositivo                     | Impossível (TPM/Enclave)               | [[Exfiltração de Dados e Chaves]]                                              |
| v1b     | TC-RNS13-01, TC-RF11-01    | Substituir chave pública/certificado ou repetir envelope | Rejeição por assinatura/cadeia         | [[Substituição de Chave (Key Substitution)]], [[Ataque de Repetição (Replay)]] |
| v1c     | TC-RF18-01                 | Usar certificado revogado                                | Rejeição imediata                      | [[Falsificação de Assinatura]]                                                 |
| v1d     | TC-RF17-03                 | Substituir `PK_case` nos metadados do caso               | Divergência com o Genesis; bloqueio    | [[Substituição de Chave (Key Substitution)]]                                   |
| v2      | TC-RNS01-01, TC-RNS22-01   | Intercetar/derrubar link satélite                        | Tráfego cifrado; degradação controlada | [[Man-in-the-Middle (MitM)]], [[Negação de Serviço (DoS)]]                     |
| v3      | TC-RNS18-01, TC-RNS12-01   | Movimento lateral a partir da quarentena                 | Bloqueio                               | [[Movimento Lateral]]                                                          |
| v4/v5   | TC-RNS14-01/02             | Downgrade ou serviço não autorizado                      | Rejeição                               | [[Ataque de Downgrade]], [[Movimento Lateral]]                                 |
| v6/v6a  | TC-RF12-01/02, TC-RNS20-01 | Adulterar/apagar logs                                    | Cadeia inválida; deteção               | [[Adulteração de Registos (Tampering)]]                                        |
| v7      | TC-RF09-01, TC-Q-04/05     | Contornar o quórum                                       | Impossível sem 3 frações               | [[Injeção de Frações (Share Injection)]]                                       |
| v8/v9   | TC-RF11-02, TC-Q-06/09     | IDOR e injeção de frações                                | Negação/validação                      | [[IDOR]], [[Injeção de Frações (Share Injection)]]                             |
| v10     | TC-RNS08-01                | Forjar assinatura                                        | Rejeição                               | [[Falsificação de Assinatura]]                                                 |
| v11     | TC-RNS02-01, TC-RNS21-01   | Exfiltrar `SK_case`/DEK/WORM                             | Impossível; zeroing                    | [[Exfiltração de Dados e Chaves]]                                              |
| v12/v14 | TC-RF15-01, TC-RF16-01     | Ler payload/notas no backend                             | Ilegível/filtrado                      | [[Exfiltração de Dados e Chaves]]                                              |
| v13     | TC-RNS15-01, TC-RF10-01    | Emitir certificado indevido                              | Negado/auditado                        | [[Escalada de Privilégios]]                                                    |
| v15/v16 | TC-RF20-01, TC-RNS04-01    | Enganar o cliente/explorar RAM                           | Verificação e zeroing                  | [[Phishing e Spoofing de Interface]], [[Exfiltração de Dados e Chaves]]        |
| vD1/vD9 | TC-RNS24-01                | Inundar a drop-box até bloquear depósitos legítimos      | Quota aplicada; recibo emitido         | [[Negação de Serviço (DoS)]]                                                   |
| vD2     | TC-RF21-02/03              | Entregar *payload* malicioso à triagem                   | Bloqueado antes da Oracle              | [[Tipos de Ataque]]                                                            |
| vD7     | TC-RF21-02                 | Evadir a sandbox (dormir, detetar VM, cifrar)            | `inconclusivo`; admissão negada        | —                                                                              |
| vD8     | TC-RNS23-01/02             | Escapar da VM ou usar a rede de análise                  | Sem montagens, sem *egress*, VM limpa  | [[Movimento Lateral]], [[Exfiltração de Dados e Chaves]]                       |
| vD10    | TC-RF21-01                 | Trocar o objeto entre análise e entrega                  | Re-hash divergente; bloqueio           | [[Adulteração de Registos (Tampering)]]                                        |
| vD11    | TC-RNS23-02, TC-RF21-03    | Enviar a prova para fora (multi-scanner, C2)             | *Egress* bloqueado; registado          | [[Exfiltração de Dados e Chaves]]                                              |

---

## 11. Evidências e relatório

Para cada caso: identificação, requisito, vetor, data, ambiente, resultado, evidência (logs, hashes, capturas), executor e assinatura. O relatório final deve incluir:

1. matriz de rastreabilidade preenchida (requisito → teste → resultado);
2. lista de desvios com severidade e mitigação;
3. riscos residuais aceites (ex.: compromisso HSM + 1 membro em RNS15);
4. parecer de conformidade, assinado pela Root CA e pelo líder operacional.

## 12. Riscos e limitações

- O ensaio de um HSM real pode ser substituído por SoftHSM; nesse caso, os testes de dupla custódia física são marcados `NOT APPLICABLE` e cobertos por inspeção de processos.
- O enclave pode ser simulado; a garantia de hardware deve ser validada em ambiente com SGX/SEV antes da produção.
- Testes de desempenho satélite dependem do perfil de órbita; L-band não é suportado para VPN e deve ser declarado fora de âmbito.
- A segurança information-theoretic é validada por análise e por testes estatísticos, não por força bruta exaustiva.
