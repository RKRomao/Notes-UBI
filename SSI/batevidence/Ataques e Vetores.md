Esta pagina dedica se a analise de vetores de ataque do projeto [[Bat (couf couf) Evidence A Wayne]]


## Revisão da arquitetura e fluxo de dados

### Fluxo 1 - Registo de Evidencias

```mermaid
sequenceDiagram
    autonumber

    actor User as User (nao admin)
    participant App as App
    participant Sat as Satelite
    participant Sys as Sistema
    participant WORM as WORM
    participant Enclave as Enclave (RAM)
    participant Users as Sistema/Users

    %% Recolha no dispositivo
    Note over User: Recolhe evidencia - caso num (representacao) - v1
    Note over User: Chave privada (em dispositivo) - v1a
    Note over User: Chave publica (em dispositivo) - v1b
    Note over User: Certificado Ed25519 - v1c
    User->>App: Envio de evidencia - v1

    %% App -> Satelite
    App->>Sat: Cifrado - evidencia (assinada), caso num, chave publica, cert Ed25519, cert WireGuard - v2

    %% Satelite -> Sistema
    Sat->>Sys: Cifrado - evidencia (assinada), caso num, chave publica, cert Ed25519, cert WireGuard - v3

    %% Interno sistema
    Sys->>Sys: Cifrado - evidencia (assinada), caso num, chave publica, cert Ed25519, cert WireGuard - v4
    Sys->>Sys: Cifrado - evidencia (assinada), caso num, chave publica, cert Ed25519, cert WireGuard -> (troca) cert mTLS - v5

    %% Registo e verificacao
    Sys->>Sys: Cifrado - Registo em log (chained) - v6a
    Note over Sys: Evidencia (assinada), chave publica (sistema vai verificar) - v7
    Note over Sys: Caso num (representacao) -> caso ID (chained)
    Note over Sys: Certificado Ed25519, cert WireGuard, assinatura - v6

    Sys->>Sys: Cifrado - evidencia (assinada) (sistema vai verificar) - chain caso solicitado - v8 / v9

    Sys->>Sys: Cifrado - Registo em log (chained) - verifica assinatura - v10

    Sys->>Sys: Cifrado - Registo em log (chained) - cria enclave (RAM) - v11
    Note over Sys: Evidencia (assinada) - guarda na chain (representacao) - guarda na WORM - v11

    %% Notificacao
    Sys->>Users: Notifica utilizadores com acesso ao caso - Registo em log (chained) - v12

    %% Resposta
    Sys->>Sat: mTLS -> gera -> certificado WireGuard, aviso de sucesso - v13
    Sat->>App: Recive, valideite, regist, decript (msg) - v14
    App->>User: Show msg - v15
```

### Fluxo 2 - Pedido de Evidencias

```mermaid
sequenceDiagram
    autonumber

    actor User as User (nao admin)
    participant App as App
    participant GW as Gateway (WireGuard/mTLS)
    participant Sys as Sistema
    participant WORM as WORM
    participant Enclave as Enclave (RAM)
    actor Quorum as Quorum

    Note over User: Gera par de chaves de sessao (efemera) - v1
    Note over User: Pedido de consulta + chave publica de sessao - v1a
    User->>App: Pedido - v1
    App->>GW: Cifrado - encaminha pedido autenticado - v2
    GW->>Sys: Cifrado - pedido autenticado, chave publica de sessao - v3

    Sys->>WORM: Cifrado - pede ficheiro cifrado e historico de notas - v4
    WORM-->>Sys: Cifrado - retorna evidencia e cadeia de anotacoes (cifradas) - v5

    Sys->>Sys: Filtra notas privadas - v6
    Sys->>Sys: Verifica integridade (Merkle Tree/Hashes) - v6a

    Sys-->>User: Cifrado - requer quorum para descifrar caso - v7
    User->>Quorum: Solicita aprovacao/chaves parciais - v8
    Quorum->>Enclave: Submissao de k chaves parciais (Shamir) - v9

    Sys->>Enclave: Cifrado - envia evidencia, notas publicas e chave publica do cliente - v10
    Enclave->>Enclave: Reconstroi chave simetrica (AES do caso) - v11
    Enclave->>Enclave: Descifra evidencia e notas publicas - v11
    Note over Enclave: Re-cifra dados com chave publica do cliente - v11
    Enclave-->>Sys: Retorna dados cifrados (ilegiveis para o backend) - v12

    Enclave->>Enclave: Apaga chaves AES e limpa RAM (zeroing) - v13

    Sys->>GW: Cifrado - envia pacote cifrado + notas pessoais (cifradas) - v14
    GW-->>User: Entrega payload totalmente encriptado - v15

    Note over User: Descifra evidencia e notas publicas (chave privada de sessao) - v16
    Note over User: Descifra notas pessoais (chave privada de identidade) - v16
```

O pedido de consulta viaja num **envelope efémero de uso único**: par X25519 novo por pedido, `request_id`, nonce de 128 bits, timestamp e TTL curto, com o *transcript* assinado em [[Ed25519]]. O túnel autentica o membro; o envelope garante *forward secrecy* e impede *replay* ou substituição da chave de resposta. Detalhe completo em [[Bat (couf couf) Evidence A Wayne#Pedidos Efémeros (Ephemeral Request Envelope)]].

## Superfície de ataque (visão geral)

```mermaid
flowchart LR
    classDef atk fill:#2b0f0f,stroke:#e74c3c,stroke-width:2px,color:#ffd6d6
    classDef vec fill:#2b1a1a,stroke:#c0392b,stroke-width:1.5px,color:#ffb3b3
    classDef sup fill:#1e1e1e,stroke:#d4a017,stroke-width:2px,color:#f0e0b0
    classDef ativ fill:#14251a,stroke:#27ae60,stroke-width:2px,color:#9be0b0

    ATK(("Atacante")):::atk

    subgraph VET ["Vetores"]
        direction TB
        V1["Phishing / engenharia social<br/>agente, jurista, membro do quórum"]:::vec
        V2["Exploração de vulnerabilidades<br/>app, gateway, backend"]:::vec
        V3["Insider threat<br/>admin do backend / operador"]:::vec
        V4["Portas e serviços expostos<br/>API, ligação satélite, HSM"]:::vec
        V5["Abuso da submissão externa<br/>mailbox pública / credencial de drop-box"]:::vec
    end

    subgraph SUP ["Superfície de ataque"]
        direction TB
        S0["Drop-box externa<br/>sandbox + quarentena"]:::sup
        S1["Dispositivo de campo<br/>chave privada, app"]:::sup
        S2["Ligação satélite<br/>WireGuard"]:::sup
        S3["Backend + SQL<br/>metadados, logs"]:::sup
        S4["Enclave + WORM<br/>RAM, object storage"]:::sup
        S5["HSM / Root CA<br/>frações Shamir"]:::sup
    end

    subgraph ATIV ["Ativos em risco"]
        direction TB
        A1["Evidência<br/>integridade + não-repúdio"]:::ativ
        A2["Chaves<br/>Ed25519 · X25519 · AES"]:::ativ
        A3["Metadados do caso<br/>quem, quando, onde"]:::ativ
        A4["Autoridade da Root CA<br/>confiança de toda a cadeia"]:::ativ
    end

    ATK --> V1 & V2 & V3 & V4 & V5
    V1 --> S1
    V1 --> S5
    V2 --> S2
    V2 --> S3
    V4 --> S2
    V4 --> S3
    V3 --> S3
    V3 --> S4
    V3 --> S5
    V5 --> S0
    V5 --> S3

    S0 --> A1
    S0 --> A3
    S1 --> A2
    S1 --> A1
    S2 --> A1
    S2 --> A3
    S3 --> A1
    S3 --> A3
    S4 --> A1
    S4 --> A2
    S5 --> A4
    S5 --> A2
```


## Vetores de Ataque e Mitigação

### Fluxo 1 — Registo de Evidências

| Vetor | Descrição                                                                             | Porque é vetor de ataque                                                                                                     | Mitigação                                                                                                                                                                                       |
| ----- | ------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| v1    | Recolha no dispositivo (evidência, caso num, chave privada/publica, cert [[Ed25519]]) | Dispositivo de campo comprometido pode expor chave privada ou alterar evidência antes da assinatura                          | Chaves em TPM/Secure Enclave (RNS10); <br>assinatura imediata no momento da apreensão; hashing SHA-256                                                                                          |
| v1a   | Chave privada no dispositivo                                                          | Exfiltração da chave privada permite impersonação e não-repúdio falso                                                        | TPM 2.0/Secure Enclave, não exportável                                                                                                                                                          |
| v1b   | Chave pública no dispositivo                                                          | Substituição da chave pública pode quebrar verificação                                                                       | Certificado [[Ed25519]] emitido pela Root CA ([[X.509]]/PKI)                                                                                                                                    |
| v1c   | Certificado [[Ed25519]] no dispositivo                                                | Certificado roubado/clonado                                                                                                  | Validação contra CRL em tempo real; validade curta                                                                                                                                              |
| v1d   | Chave pública do caso (`PK_case`) nos metadados do caso                               | Substituição de `PK_case` faz com que os depósitos seguintes sejam envolvidos para o atacante, que passa a poder decifrá-los | `PK_case` comprometida no Bloco 0 (`SHA-256(PK_case ‖ case_id)`); o cliente compara o valor recebido com o do Genesis; alteração exige quebrar o encadeamento de hashes e a assinatura do bloco |
| v2    | App → Satélite (cifrado)                                                              | Interceptação/MitM no link satélite; degradação de performance (PEP)                                                         | [[WireGuard]] (ChaCha20-Poly1305); <br>MTU/MSS tuning; keepalive                                                                                                                                |
| v3    | Satélite → Sistema                                                                    | Comprometimento do satélite ou gateway                                                                                       | [[mTLS]] com certificados [[X.509]]; <br>validação de certificado                                                                                                                               |
| v4    | Sistema interno (1.ª passagem)                                                        | Movimento lateral interno; acesso não autorizado                                                                             | [[mTLS]] entre serviços; menor privilégio                                                                                                                                                       |
| v5    | Sistema interno (troca para [[mTLS]])                                                 | Downgrade de algoritmo ou troca maliciosa de certificado                                                                     | Lista de algoritmos permitidos; <br>verificador rejeita fora da lista                                                                                                                           |
| v6    | Registo em log (chained) + verificação                                                | Falsificação de logs ou quebra da cadeia                                                                                     | Hash-chaining (prev_hash); assinatura [[Ed25519]]; WORM                                                                                                                                         |
| v6a   | Registo em log chained                                                                | Reescrita de logs de auditoria                                                                                               | Blockchain sidechain imutável (T_AUDIT_BLOCK)                                                                                                                                                   |
| v7    | Sistema verifica assinatura                                                           | Bypass da verificação ou aceitação de algoritmo fraco                                                                        | Verificação obrigatória; <br>lista de algoritmos permitidos                                                                                                                                     |
| v8    | Chain caso solicitado                                                                 | Acesso a caso sem autorização                                                                                                | Controlo de acesso por caso; <br>hierarquia de acesso                                                                                                                                           |
| v9    | Chain caso solicitado (sub-passo)                                                     | Enumeração de casos ou IDOR                                                                                                  | Autorização por caso; auditoria global                                                                                                                                                          |
| v10   | Verificação de assinatura                                                             | Aceitação de assinatura forjada                                                                                              | Verificação [[Ed25519]] contra certificado válido                                                                                                                                               |
| v11   | Criação de enclave (RAM) + guarda WORM                                                | Exfiltração de `SK_case` ou da DEK da RAM; corrupção do WORM                                                                 | Zeroization (RNS04); reconstituição só em RAM, sem swap nem core dump; WORM append-only (RNS05)                                                                                                 |
| v12   | Notificação a utilizadores                                                            | Fuga de informação sobre quem tem acesso                                                                                     | Notificação apenas a utilizadores autorizados; log chained                                                                                                                                      |
| v13   | Sistema → Satélite ([[mTLS]], gera cert [[WireGuard]])                                | Emissão de certificado a entidade não autorizada                                                                             | Root CA (Alfred, HSM offline); <br>CRL                                                                                                                                                          |
| v14   | Satélite → App (receive, validate, decrypt)                                           | Payload malicioso entregue ao cliente                                                                                        | Validação no cliente; decriptação apenas com chave privada                                                                                                                                      |
| v15   | App → User (show msg)                                                                 | Exibição de dados não verificados; phishing de UI                                                                            | Verificação de integridade antes de apresentar                                                                                                                                                  |

### Fluxo 2 — Pedido de Evidências

| Vetor | Descrição                                                       | Porque é vetor de ataque                                                          | Mitigação                                                                                                                                 |
| ----- | --------------------------------------------------------------- | --------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| v1    | Geração de par de chaves de sessão efémero                      | Chaves de sessão fracas, reutilizadas ou correlacionáveis                         | Um par X25519 por pedido, só em RAM;<br>zeroing após descifragem (RNS04/RNS13)                                                            |
| v1a   | Pedido + chave pública de sessão                                | Substituição da chave pública de sessão (*key substitution*) pelo backend ou MitM | Assinatura [[Ed25519]] do *transcript* (`request_id`, caso, nonce, timestamp, pk_efémera);<br>lido dentro do túnel [[WireGuard]]/[[mTLS]] |
| v1b   | Nonce + timestamp + TTL do envelope                             | Replay de um pedido capturado para obter nova resposta                            | Nonce de 128 bits com cache de *hash* até ao TTL;<br>janela de relógio ±60 s; *fail-closed*                                               |
| v2    | App → Gateway                                                   | Interceptação no link de campo                                                    | [[WireGuard]] (ChaCha20-Poly1305)                                                                                                         |
| v3    | Gateway → Sistema                                               | Acesso não autorizado ao backend                                                  | [[mTLS]]; <br>autenticação prévia                                                                                                         |
| v4    | Sistema → WORM (pedido)                                         | Acesso indevido ao WORM                                                           | Backend como único intermediário; <br>WORM cego                                                                                           |
| v5    | WORM → Sistema (resposta cifrada)                               | Exfiltração do binário cifrado                                                    | Binário ilegível sem a DEK; <br>DEK só existe envolta para `PK_case`                                                                     |
| v6    | Filtragem de notas privadas                                     | Fuga de notas privadas a utilizador não autorizado                                | Filtragem no backend antes da entrega                                                                                                     |
| v6a   | Verificação de integridade ([[Árvore de Merkle Clássica (Dense)]]) | Aceitação de evidência adulterada                                                 | Verificação Merkle/Hashes obrigatória                                                                                                     |
| v7    | Sistema → User (requer quórum)                                  | Bypass do quórum; acesso sem autorização                                          | Quórum obrigatório para decifração (k=3: 2 do Alfred + 1 membro, ou 3 membros)                                                            |
| v8    | User → Quórum (solicita chaves)                                 | Solicitação fraudulenta de frações                                                | Autenticação do solicitante; <br>auditoria                                                                                                |
| v9    | Quórum → Enclave (submissão Shamir)                             | Injeção de frações falsas                                                         | Validação das frações; <br>frações do Alfred libertadas apenas pelo HSM; <br>enclave isolado                                              |
| v9a   | Frações do Alfred no HSM (2 frações)                            | Compromisso do HSM + 1 membro reduz o limiar efetivo a 2 pessoas                  | Duplo controlo físico no HSM; <br>libertação apenas com ordem de caso assinada; <br>auditoria do QUORUM_INIT                              |
| v10   | Sistema → Enclave (envia evidência + chave pública cliente)     | Injeção de dados maliciosos no enclave                                            | Enclave recebe apenas dados necessários; <br>backend não decifra                                                                          |
| v11   | Reconstituição + decifração + re-cifragem no enclave            | Exfiltração de `SK_case` ou dos dados em claro da RAM                             | Zeroization (RNS04); <br>reconstituição apenas em RAM do enclave; backend nunca vê o texto simples                                        |
| v12   | Enclave → Sistema (dados cifrados)                              | Interceptação do pacote cifrado                                                   | Cifrado com chave pública do cliente; <br>ilegível para backend                                                                           |
| v13   | Zeroization no enclave                                          | Falha na limpeza da RAM                                                           | Memory wipe forçado (RNS04);<br>verificação                                                                                               |
| v14   | Sistema → Gateway (pacote cifrado + notas pessoais)             | Fuga de notas pessoais                                                            | Notas pessoais decifradas com chave privada de identidade                                                                                 |
| v15   | Gateway → User (entrega)                                        | Interceptação no último salto                                                     | [[WireGuard]]; <br>payload já cifrado E2EE                                                                                                |
| v16   | Descifração no cliente                                          | Comprometimento do cliente; chave privada exposta                                 | Chave privada em TPM/Secure Enclave; zeroization local                                                                                    |

### Drop-box externa (GCPD) — vetores vD1–vD6

| Vetor | Descrição                                                       | Porque é vetor de ataque                                                                                             | Mitigação                                                                                                                              |
| ----- | --------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| vD1   | Submissão em massa / *flooding* da mailbox pública              | Esgota *storage* e a fila de triagem da Oracle (DoS lógico), escondendo um depósito legítimo                         | Quota por identidade e por caso; *rate limit* no gateway; limite de tamanho; recusa *fail-closed* acima do limite; alerta na auditoria |
| vD2   | *Payload* malicioso (executável, macro, *zip bomb*, *polyglot*) | A Oracle abre o artefacto numa estação com acesso ao interior → RCE ou *phishing* ao analista                        | Buffer isolado sem ligação à BD central; AV/CDR na entrada; triagem em VM descartável; nunca abrir no *host*                           |
| vD3   | Abuso do recibo (hash + timestamp)                              | O recibo prova que **algo** foi depositado, não que o conteúdo é autêntico; pode ser invocado como prova de custódia | Recibo assinado cobre apenas hash e timestamp; não atesta veracidade; verificação em [[Árvore de Merkle Clássica (Dense)]]             |
| vD4   | Quarentena como trampolim para o interior                       | Se a quarentena partilhar rede ou serviços com o backend, o atacante move-se lateralmente ([[Movimento Lateral]])    | DMZ dedicada; *firewall deny-by-default*; apenas o gateway de quarentena alcançável                                                    |
| vD5   | URI forjado / *path traversal* no depósito                      | Escrita fora do prefixo do caso ou sobrescrita de objeto no buffer                                                   | URI gerado pelo servidor; validação canónica; prefixo por caso; escrita *write-once*                                                   |
| vD6   | Repudiação do depósito ou credencial partilhada                 | Sem autenticação forte do remetente, o depósito é anónimo; a credencial institucional pode ser reutilizada           | *Leaf* [[Ed25519]] `GCPD-CLI-001` (6 meses) + [[mTLS]] no gateway externo; assinatura do manifesto; CRL e rotação                      |

### Sandbox de detonação — vetores vD7–vD11

| Vetor | Descrição                                                                                                                                           | Porque é vetor de ataque                                                   | Mitigação                                                                                                                                                                                                               |
| ----- | --------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| vD7   | **Evasão da sandbox** — *malware* que dorme mais que a janela de análise, deteta artefactos de VM, exige interação, ou vem em arquivo cifrado/opaco | Produz um veredicto `limpo` falso e a prova segue para admissão            | Estático (tipo real, entropia, YARA) + dinâmico; atraso e *jitter*; janela alargada para amostras suspeitas; arquivos cifrados ficam `inconclusivo` até a Oracle fornecer a palavra-passe; CDR quando a prova o permite |
| vD8   | **Escape da sandbox** — *exploit* do hipervisor ou do kernel, montagens partilhadas, canal lateral                                                  | Comprometer o host de análise e, a partir dele, a rede de quarentena       | MicroVM com kernel próprio e imagem imutável; sem credenciais nem chaves dentro da VM; VLAN de análise sem rotas; host dedicado; hipervisor atualizado; VM destruída a cada amostra (RNS23)                             |
| vD9   | **DoS do pipeline** — *zip bomb*, ficheiro enorme, detonação longa, fila cheia                                                                      | Atrasa todos os depósitos e, num caso urgente, faz perder prova            | Caps *antes* da detonação (tamanho, profundidade, rácio de compressão); *timeout* rígido; fila limitada com *backpressure*; recibo emitido independentemente do veredicto (RF22/RNS24)                                  |
| vD10  | **TOCTOU análise ↔ entrega** — o objeto detonado não é o objeto guardado (re-upload, *dedupe* por nome, substituição no buffer)                     | Entra no perímetro uma prova que nunca foi analisada                       | Hash calculado no gateway; a mesma cópia é selada; veredicto indexado por `SHA-256`; re-hash no momento em que a Oracle abre; escrita *write-once*                                                                      |
| vD11  | **Fuga da prova para fora** — *egress* da sandbox, envio para multi-scanner *online*, telemetria                                                    | Quebra a confidencialidade e a cadeia de custódia sem deixar rasto interno | *Egress* `deny-by-default`; só o *sinkhole* local; motores e regras locais; proibição técnica e contratual de upload externo; registo de todo o tráfego da VLAN                                                         |
### Defesa na drop-box
Para além da sandbox, o próprio ponto de entrada tem de estar endurecido. Cada controlo fecha um vetor concreto:

| Controlo | Fecha |
| --- | --- |
| Identidade **por pessoa** no GCPD, certificado de 6 meses, CRL em tempo real | vD6 — deixa de haver credencial institucional partilhada |
| Quota por identidade e por caso, *rate limit*, limite de tamanho, lista de tipos | vD1, vD9 |
| Hash e recibo emitidos no gateway, ligados ao objeto efetivamente guardado | vD3, vD10 |
| URI gerado pelo servidor, *write-once*, sem listagem de objetos | vD5 |
| Buffer cifrado em repouso, com chave do serviço de quarentena e rotação | vD2, vD4 |
| Montagem `noexec,nosuid,nodev` no buffer; nada é executado fora da sandbox | vD2 |
| Auditoria das submissões **recusadas**, não só das aceites | vD1, vD5 |
| Relógio fidedigno (±60 s, RNS16) em recibos e veredictos | vD3 |
| Leitura da Oracle apenas *read-only*, com DLP e registo de cada acesso | vD4, vD10 |
| *Fail-closed* na admissão, mas depósito continua aceite e com recibo | vD9 (disponibilidade sem perder custódia) |

## Diagramas de ataque

> [!info] Como ler
> - **Vermelho tracejado** = movimento do atacante; a etiqueta indica o vetor (`vN` da tabela acima, `vD*` da secção da drop-box).
> - **Setas cheias** = fluxo legítimo do sistema; seta tracejada cinzenta com "(bloqueado)" = tentativa travada pela segmentação.
> - **Verde** = ativo ou controlo que fecha o vetor; **azul** = backend; **amarelo** = transporte; **roxo** = zona sensível (ledger, enclave); **cinzento tracejado** = armazenamento cego.

### Fluxo 1 — Registo de evidências

```mermaid
flowchart LR
    classDef atk fill:#2b0f0f,stroke:#e74c3c,stroke-width:2px,color:#ffd6d6
    classDef campo fill:#1e1e1e,stroke:#27ae60,stroke-width:2px,color:#e0e0e0
    classDef trans fill:#1a1a1a,stroke:#d4a017,stroke-width:2px,color:#f0e0b0
    classDef back fill:#1e1e1e,stroke:#3498db,stroke-width:2px,color:#d6eaf8
    classDef sens fill:#2b2b2b,stroke:#8b6ce3,stroke-width:2px,color:#ffffff
    classDef store fill:#1a1a1a,stroke:#5c5c5c,stroke-width:2px,color:#aaaaaa,stroke-dasharray:5 5

    ATK(("Atacante")):::atk

    subgraph EXTERNA ["0 · Entrada externa (RF04 / RF05)"]
        direction TB
        GCPD["Comissário Gordon<br/>GCPD-CLI-001 · write-only"]:::campo
        DROP["Drop-box<br/>buffer isolado"]:::store
        SBX["Sandbox<br/>microVM descartável"]:::store
        ORACLE["Oracle<br/>triagem manual"]:::campo
    end

    subgraph CAMPO ["1 · Campo"]
        direction TB
        U["User<br/>recolhe e assina"]:::campo
        APP["App<br/>dispositivo do agente"]:::campo
    end

    subgraph LINK ["2 · Satélite"]
        direction TB
        SAT["Satélite / Gateway<br/>WireGuard"]:::trans
    end

    subgraph BACK ["3 · Backend"]
        direction TB
        SYS["Sistema<br/>registo e verificação"]:::back
        SVC["Serviços internos<br/>mTLS, menor privilégio"]:::back
        LOG["Log chained<br/>T_AUDIT_BLOCK"]:::sens
        CASECH["Chain do caso<br/>T_CASE_BLOCK"]:::sens
    end

    subgraph SENSIVEL ["4 · Zona sensível"]
        direction TB
        ENC["Enclave (RAM)<br/>zeroization"]:::sens
        WORM["WORM<br/>append-only"]:::store
    end

    USERS["5 · Utilizadores com acesso"]:::campo

    U -->|"v1"| APP
    APP -->|"v2"| SAT
    SAT -->|"v3"| SYS
    GCPD -->|"vD6"| DROP
    DROP -->|"vD2 · vD7 detonação"| SBX
    SBX -->|"veredicto assinado"| ORACLE
    ORACLE -->|"admissão RF05"| SYS
    SYS -->|"v4 → v5"| SVC
    SVC -->|"v6 · v6a"| LOG
    SVC -->|"v7 · v8 · v9 · v10"| CASECH
    CASECH -->|"v11"| ENC
    ENC --> WORM
    SVC -->|"v12"| USERS
    SVC -.->|"v13 mTLS"| SAT
    SAT -.->|"v14"| APP
    APP -.->|"v15"| U

    ATK -.->|"v1a · v1b · v1c"| U
    ATK -.->|"v1 evidência antes da assinatura"| APP
    ATK -.->|"v2 MitM no link"| SAT
    ATK -.->|"v3 satélite comprometido"| SYS
    ATK -.->|"v4 · v5 movimento lateral"| SVC
    ATK -.->|"v6 · v6a reescrita de logs"| LOG
    ATK -.->|"v8 · v9 IDOR / enumeração"| CASECH
    ATK -.->|"v10 · v11 injeção no enclave"| ENC
    ATK -.->|"v12 fuga de acessos"| USERS
    ATK -.->|"vD1 · vD4 drop-box"| DROP
    ATK -.->|"vD8 escape da sandbox"| SBX
```


### Drop-box externa (GCPD)

A drop-box é o único caminho de entrada para quem está fora da VPN: o GCPD deposita às cegas (`GCPD-CLI-001`, write-only), o artefacto é pré-filtrado e detonado numa sandbox, e só depois é gravado num buffer exterior cifrado, sem ligação à BD central, até a Oracle o admitir (RF04, RF05, RF21). É por isso uma superfície de ataque própria — não é o `v3` do satélite nem o `v1` do terreno, embora a rastreabilidade em [[Intregavel 4]] a agregue a `v3`/`TC-RNS18-01`.

**Vista de fluxo** — o caminho legítimo e os pontos onde o atacante entra:

```mermaid
flowchart LR
    classDef atk fill:#2b0f0f,stroke:#e74c3c,stroke-width:2px,color:#ffd6d6
    classDef ext fill:#1e1e1e,stroke:#d4a017,stroke-width:2px,color:#f0e0b0
    classDef quarent fill:#1a1a1a,stroke:#5c5c5c,stroke-width:2px,color:#aaaaaa,stroke-dasharray:5 5
    classDef store fill:#1a1a1a,stroke:#5c5c5c,stroke-width:2px,color:#aaaaaa,stroke-dasharray:5 5
    classDef back fill:#1e1e1e,stroke:#3498db,stroke-width:2px,color:#d6eaf8

    ATK(("Atacante")):::atk

    GCPD["Comissário Gordon / GCPD<br/>GCPD-CLI-001 · write-only"]:::ext
    ING["Gateway de submissão<br/>mTLS · quota · tamanho"]:::ext
    PRE["Pré-filtro estático<br/>tipo real · caps · hash"]:::ext
    SBX["Sandbox de detonação<br/>microVM · sem egress"]:::quarent
    SINK["Sinkhole<br/>Internet simulada"]:::store
    VER["Veredicto assinado<br/>ligado ao SHA-256"]:::quarent
    Q["Quarentena<br/>buffer cifrado"]:::store
    ORC["Oracle<br/>triagem e admissão RF05"]:::back
    SYS["Backend interno"]:::back

    GCPD -->|"vD6"| ING
    ING -->|"vD1 · vD5"| PRE
    PRE -->|"vD9"| SBX
    SBX --> VER
    VER -->|"vD10"| Q
    Q --> ORC
    ORC -->|"admissão assinada"| SYS
    ING -.->|"vD3 recibo (hash + timestamp)"| GCPD
    SBX -.->|"vD11 egress só para o sinkhole"| SINK
    Q -.->|"vD4 bloqueado por segmentação"| SYS

    ATK -.->|"vD1 flooding / quota"| ING
    ATK -.->|"vD2 payload malicioso"| Q
    ATK -.->|"vD7 evasão · vD8 escape"| SBX
    ATK -.->|"vD10 troca análise ↔ entrega"| VER
    ATK -.->|"vD4 trampolim para o interior"| Q
```

**Árvore de ataque** — cada linha é *vetor → controlo → impacto se o controlo falhar*. O atacante nunca chega ao backend: o pior caso tem de ser parado dentro da DMZ.

```mermaid
flowchart LR
    classDef atk fill:#2b0f0f,stroke:#e74c3c,stroke-width:2px,color:#ffd6d6
    classDef vec fill:#2b1a1a,stroke:#c0392b,stroke-width:1.5px,color:#ffb3b3
    classDef ctl fill:#14251a,stroke:#27ae60,stroke-width:2px,color:#9be0b0
    classDef alvo fill:#1e1e1e,stroke:#d4a017,stroke-width:1.5px,color:#f0e0b0

    ATK(("Atacante<br/>(externo ou parceiro)")):::atk

    D1["vD1 Flooding"]:::vec
    D2["vD2 Payload"]:::vec
    D3["vD3 Recibo"]:::vec
    D4["vD4 Trampolim"]:::vec
    D5["vD5 URI"]:::vec
    D6["vD6 Repudiação"]:::vec

    C1["Quota · rate limit<br/>· limite de tamanho"]:::ctl
    C2["Sandbox microVM<br/>· AV/YARA · CDR"]:::ctl
    C3["Recibo assinado<br/>só hash + timestamp"]:::ctl
    C4["DMZ · deny-by-default<br/>só o gateway exposto"]:::ctl
    C5["URI gerado no servidor<br/>· write-once"]:::ctl
    C6["mTLS · Ed25519<br/>· CRL e rotação"]:::ctl

    A1["Storage e fila<br/>esgotados"]:::alvo
    A2["RCE na estação<br/>da Oracle"]:::alvo
    A3["Custódia invocada<br/>sem prova"]:::alvo
    A4["Movimento lateral<br/>para o interior"]:::alvo
    A5["Objeto sobrescrito<br/>no buffer"]:::alvo
    A6["Depósito anónimo<br/>ou repudiado"]:::alvo

    ATK --> D1 & D2 & D3 & D4 & D5 & D6

    D1 --> C1 --> A1
    D2 --> C2 --> A2
    D3 --> C3 --> A3
    D4 --> C4 --> A4
    D5 --> C5 --> A5
    D6 --> C6 --> A6
```

**Leitura:** a drop-box fecha a **confidencialidade** por construção (write-only, sem leitura e sem ligação à BD), por isso os vetores que restam atacam outros três eixos — **disponibilidade** (vD1), **integridade do host de triagem** (vD2, vD5) e **valor probatório** (vD3, vD6), com a **contenção** (vD4) como condição para que nenhum deles escale ao backend.


### Sandbox de detonação

A sandbox resolve o buraco do `vD2`: em vez de a Oracle abrir o ficheiro, o ficheiro é **detonado** primeiro e só entra no buffer depois de existir veredicto. Duas regras de desenho que não se podem perder:

- **O veredicto é indexado pelo `SHA-256` do objeto armazenado**, não pelo nome nem por um upload paralelo — senão há TOCTOU (`vD10`).
- **"Sem deteção" ≠ "seguro".** A sandbox só produz três estados: `malicioso`, `limpo` e `inconclusivo`. `inconclusivo` bloqueia admissão tal como `malicioso`; a decisão final continua a ser humana (RF05).

```mermaid
flowchart LR
    classDef atk fill:#2b0f0f,stroke:#e74c3c,stroke-width:2px,color:#ffd6d6
    classDef ext fill:#1e1e1e,stroke:#d4a017,stroke-width:2px,color:#f0e0b0
    classDef quarent fill:#1a1a1a,stroke:#5c5c5c,stroke-width:2px,color:#aaaaaa,stroke-dasharray:5 5
    classDef back fill:#1e1e1e,stroke:#3498db,stroke-width:2px,color:#d6eaf8

    ATK(("Atacante")):::atk

    subgraph DMZ ["DMZ da drop-box"]
        direction TB
        G["Gateway<br/>mTLS · quota · tamanho"]:::ext
        P["Pré-filtro estático<br/>tipo real · caps · hash"]:::ext
        S["Sandbox<br/>microVM descartável"]:::quarent
        V["Veredicto assinado<br/>SHA-256 + YARA"]:::quarent
    end

    subgraph ANALISE ["VLAN de análise (sem rotas para o interior)"]
        direction TB
        K["Sinkhole / INetSim<br/>Internet simulada"]:::quarent
    end

    subgraph BUF ["Buffer exterior"]
        direction TB
        Q["Quarentena cifrada<br/>write-once"]:::quarent
    end

    subgraph INT ["Perímetro interno"]
        direction TB
        O["Oracle (read-only)<br/>admissão RF05"]:::back
        SYS["Backend + WORM"]:::back
    end

    G --> P --> S
    S --> V --> Q
    S -.->|"única saída permitida"| K
    Q -->|"credencial restrita"| O
    O -->|"admissão assinada"| SYS

    ATK -.->|"vD9 zip bomb / fila"| P
    ATK -.->|"vD7 evasão · vD8 escape"| S
    ATK -.->|"vD10 TOCTOU"| V
    ATK -.->|"vD11 exfiltração"| K
```

### Fluxo 2 — Pedido de evidências

```mermaid
flowchart LR
    classDef atk fill:#2b0f0f,stroke:#e74c3c,stroke-width:2px,color:#ffd6d6
    classDef cli fill:#1e1e1e,stroke:#27ae60,stroke-width:2px,color:#e0e0e0
    classDef trans fill:#1a1a1a,stroke:#d4a017,stroke-width:2px,color:#f0e0b0
    classDef back fill:#1e1e1e,stroke:#3498db,stroke-width:2px,color:#d6eaf8
    classDef sens fill:#2b2b2b,stroke:#8b6ce3,stroke-width:2px,color:#ffffff
    classDef store fill:#1a1a1a,stroke:#5c5c5c,stroke-width:2px,color:#aaaaaa,stroke-dasharray:5 5

    ATK(("Atacante")):::atk

    subgraph CLIENTE ["1 · Cliente"]
        direction TB
        U["User<br/>par X25519 efémero (RAM)"]:::cli
        APP["App<br/>envelope assinado<br/>request_id · nonce · ts · TTL"]:::cli
        OUT["Payload E2EE<br/>E2EE contra o backend"]:::cli
        U2["User<br/>descifra sessão + identidade"]:::cli
    end

    subgraph GWZ ["2 · Gateway"]
        direction TB
        GW["Gateway<br/>WireGuard + mTLS"]:::trans
    end

    subgraph BACK ["3 · Backend"]
        direction TB
        SYS["Sistema<br/>filtra notas privadas<br/>verifica Merkle"]:::back
        WORM["WORM<br/>evidência + notas cifradas"]:::store
    end

    subgraph SENSIVEL ["4 · Zona sensível"]
        direction TB
        Q["Quórum (+ HSM)<br/>k frações Shamir"]:::sens
        ENC["Enclave (RAM)<br/>reconstrói EK<br/>decifra · re-cifra · zeroing"]:::sens
    end

    U -->|"v1 · v1a"| APP
    APP -->|"v1b"| GW
    GW -->|"v2 · v3"| SYS
    SYS -->|"v4"| WORM
    WORM -.->|"v5"| SYS
    SYS -->|"v7"| Q
    U -->|"v8"| Q
    Q -->|"v9 · v9a"| ENC
    SYS -->|"v10"| ENC
    ENC -->|"v12"| OUT
    OUT -->|"v14"| GW
    GW -->|"v15"| U2

    ATK -.->|"v1 chaves fracas / reutilizadas"| U
    ATK -.->|"v1a key substitution"| APP
    ATK -.->|"v1b · v2 replay / interceção"| GW
    ATK -.->|"v3 · v6 · v6a backend"| SYS
    ATK -.->|"v4 · v5 WORM"| WORM
    ATK -.->|"v7 · v8 · v9 · v9a quórum"| Q
    ATK -.->|"v10 · v11 · v13 RAM do enclave"| ENC
    ATK -.->|"v14 · v15 · v16 entrega e cliente"| OUT
```


## Rastreabilidade para os testes ([[Intregavel 4]])

Cada vetor tem um caso de teste associado no plano de testes. A tabela seguinte é o índice rápido; o detalhe do procedimento e do critério de aceitação está em [[Intregavel 4#10. Testes adversariais e mapeamento aos vetores]].

| Vetor    | Caso(s) de teste                                                   |
| -------- | ------------------------------------------------------------------ |
| v1, v1a  | TC-RNS10-01                                                        |
| v1b      | TC-RF11-01, TC-RNS13-01                                            |
| v1c      | TC-RF18-01                                                         |
| v1d      | TC-RF17-01, TC-RF17-02                                             |
| v2       | TC-RNS01-01, TC-RNS22-01                                           |
| v3       | TC-RNS12-01, TC-RNS18-01                                           |
| v4, v5   | TC-RNS14-01, TC-RNS14-02                                           |
| v6, v6a  | TC-RF12-01, TC-RF12-02, TC-RNS20-01                                |
| v7       | TC-RF09-01, TC-Q-04, TC-Q-05                                       |
| v8, v9   | TC-RF11-02, TC-Q-06, TC-Q-09                                       |
| v10      | TC-RNS08-01                                                        |
| v11      | TC-RNS02-01, TC-RNS21-01                                           |
| v12, v14 | TC-RF15-01, TC-RF16-01                                             |
| v13      | TC-RNS15-01, TC-RF10-01                                            |
| v15, v16 | TC-RF20-01, TC-RNS04-01                                            |
| vD1, vD5 | TC-RF04-01 (sem caso dedicado para quota/URI — sugerir TC-RF04-02) |
| vD2, vD3 | TC-RF05-01; recibo em TC-RF20-01                                   |
| vD4      | TC-RNS18-01 (é o sentido de "v3" na matriz do Intregavel 4)        |
| vD6      | TC-RF04-01, TC-RF18-01                                             |
| vD7      | TC-RF21-02 (evasão); TC-RF21-03 (arquivo cifrado → inconclusivo)   |
| vD8      | TC-RNS23-01 (escape/isolamento)                                    |
| vD9      | TC-RNS24-01 (quota, fila, *backpressure*)                          |
| vD10     | TC-RF21-01 (veredicto ligado ao SHA-256), TC-RF21-03               |
| vD11     | TC-RNS23-02 (*egress* bloqueado)                                   |

> [!note] Numeração
> Os vetores `vD*` são próprios da drop-box e não colidem com `v1`–`v16`, para não partir a matriz de rastreabilidade já congelada. No [[Intregavel 4]] aparecem agregados a **v3** (RF04, RF05, RNS18); se preferires, renumero-os para `v3a`–`v3f` e atualizo as duas notas.


> [!warning] Leitura rápida
> No **Fluxo 1** o elo fraco é o **campo**: a evidência existe em claro no dispositivo antes da assinatura (v1) e a chave privada vive lá (v1a). No **Fluxo 2** o elo fraco é o **envelope**: quem controla o backend ou o gateway pode tentar substituir a chave efémera (v1a) ou fazer replay (v1b) — daí a assinatura [[Ed25519]] do *transcript* e o nonce com cache até ao TTL. Em ambos os fluxos o enclave é o único ponto que vê o texto simples e a `SK_case` reconstruída (v11), e o HSM é o único ponto que liberta as frações do Alfred (v9a). A **drop-box** acrescenta um terceiro elo: fechada a leitura (write-only), o risco desloca-se para o abuso do canal — *flooding* (vD1), *payload* malicioso (vD2) e quarentena usada como trampolim (vD4). Com a **sandbox**, o risco volta a mudar de sítio: já não é a Oracle que abre o ficheiro, mas a própria análise que pode ser enganada (vD7), explorada (vD8), saturada (vD9), desalinhada do objeto guardado (vD10) ou usada para exfiltrar a prova (vD11).

