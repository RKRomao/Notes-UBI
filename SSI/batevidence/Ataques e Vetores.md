Esta pagina dedica se a analise de vetores de ataque do projeto [[Bat (couf couf) Evidence A Wayne]]

## Revisão da arquitetura e fluxo de dados

### Fluxo 1 — Registo de Evidencias

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

### Fluxo 2 — Pedido de Evidencias

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

## Vetores de Ataque e Mitigação

### Fluxo 1 — Registo de Evidências

| Vetor | Descrição                                                                             | Porque é vetor de ataque                                                                            | Mitigação                                                                                              |
| ----- | ------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------ |
| v1    | Recolha no dispositivo (evidência, caso num, chave privada/publica, cert [[Ed25519]]) | Dispositivo de campo comprometido pode expor chave privada ou alterar evidência antes da assinatura | Chaves em TPM/Secure Enclave (RNS10); <br>assinatura imediata no momento da apreensão; hashing SHA-256 |
| v1a   | Chave privada no dispositivo                                                          | Exfiltração da chave privada permite impersonação e não-repúdio falso                               | TPM 2.0/Secure Enclave, não exportável                                                                 |
| v1b   | Chave pública no dispositivo                                                          | Substituição da chave pública pode quebrar verificação                                              | Certificado [[Ed25519]] emitido pela Root CA ([[X.509]]/PKI)                                           |
| v1c   | Certificado [[Ed25519]] no dispositivo                                                | Certificado roubado/clonado                                                                         | Validação contra CRL em tempo real; validade curta                                                     |
| v2    | App → Satélite (cifrado)                                                              | Interceptação/MitM no link satélite; degradação de performance (PEP)                                | [[WireGuard]] (ChaCha20-Poly1305); <br>MTU/MSS tuning; keepalive                                       |
| v3    | Satélite → Sistema                                                                    | Comprometimento do satélite ou gateway                                                              | [[mTLS]] com certificados [[X.509]]; <br>validação de certificado                                      |
| v4    | Sistema interno (1.ª passagem)                                                        | Movimento lateral interno; acesso não autorizado                                                    | [[mTLS]] entre serviços; menor privilégio                                                              |
| v5    | Sistema interno (troca para [[mTLS]])                                                 | Downgrade de algoritmo ou troca maliciosa de certificado                                            | Lista de algoritmos permitidos; <br>verificador rejeita fora da lista                                  |
| v6    | Registo em log (chained) + verificação                                                | Falsificação de logs ou quebra da cadeia                                                            | Hash-chaining (prev_hash); assinatura [[Ed25519]]; WORM                                                |
| v6a   | Registo em log chained                                                                | Reescrita de logs de auditoria                                                                      | Blockchain sidechain imutável (T_AUDIT_BLOCK)                                                          |
| v7    | Sistema verifica assinatura                                                           | Bypass da verificação ou aceitação de algoritmo fraco                                               | Verificação obrigatória; <br>lista de algoritmos permitidos                                            |
| v8    | Chain caso solicitado                                                                 | Acesso a caso sem autorização                                                                       | Controlo de acesso por caso; <br>hierarquia de acesso                                                  |
| v9    | Chain caso solicitado (sub-passo)                                                     | Enumeração de casos ou IDOR                                                                         | Autorização por caso; auditoria global                                                                 |
| v10   | Verificação de assinatura                                                             | Aceitação de assinatura forjada                                                                     | Verificação [[Ed25519]] contra certificado válido                                                      |
| v11   | Criação de enclave (RAM) + guarda WORM                                                | Exfiltração da chave AES da RAM; corrupção do WORM                                                  | Zeroization (RNS04); WORM append-only (RNS05)                                                          |
| v12   | Notificação a utilizadores                                                            | Fuga de informação sobre quem tem acesso                                                            | Notificação apenas a utilizadores autorizados; log chained                                             |
| v13   | Sistema → Satélite ([[mTLS]], gera cert [[WireGuard]])                                | Emissão de certificado a entidade não autorizada                                                    | Root CA (Alfred, HSM offline); <br>CRL                                                                 |
| v14   | Satélite → App (receive, validate, decrypt)                                           | Payload malicioso entregue ao cliente                                                               | Validação no cliente; decriptação apenas com chave privada                                             |
| v15   | App → User (show msg)                                                                 | Exibição de dados não verificados; phishing de UI                                                   | Verificação de integridade antes de apresentar                                                         |

### Fluxo 2 — Pedido de Evidências

| Vetor | Descrição                                                       | Porque é vetor de ataque                           | Mitigação                                                        |
| ----- | --------------------------------------------------------------- | -------------------------------------------------- | ---------------------------------------------------------------- |
| v1    | Geração de par de chaves de sessão efémero                      | Chaves de sessão fracas, reutilizadas ou correlacionáveis | Um par X25519 por pedido, só em RAM;<br>zeroing após descifragem (RNS04/RNS13) |
| v1a   | Pedido + chave pública de sessão                                | Substituição da chave pública de sessão (*key substitution*) pelo backend ou MitM | Assinatura [[Ed25519]] do *transcript* (`request_id`, caso, nonce, timestamp, pk_efémera);<br>lido dentro do túnel [[WireGuard]]/[[mTLS]] |
| v1b   | Nonce + timestamp + TTL do envelope                             | Replay de um pedido capturado para obter nova resposta | Nonce de 128 bits com cache de *hash* até ao TTL;<br>janela de relógio ±60 s; *fail-closed* |
| v2    | App → Gateway                                                   | Interceptação no link de campo                     | [[WireGuard]] (ChaCha20-Poly1305)                                |
| v3    | Gateway → Sistema                                               | Acesso não autorizado ao backend                   | [[mTLS]]; <br>autenticação prévia                                |
| v4    | Sistema → WORM (pedido)                                         | Acesso indevido ao WORM                            | Backend como único intermediário; <br>WORM cego                  |
| v5    | WORM → Sistema (resposta cifrada)                               | Exfiltração do binário cifrado                     | Binário ilegível sem EK; <br>EK só no enclave                    |
| v6    | Filtragem de notas privadas                                     | Fuga de notas privadas a utilizador não autorizado | Filtragem no backend antes da entrega                            |
| v6a   | Verificação de integridade ([[Árvore de Merkle Clássica (Dense) | Aceitação de evidência adulterada                  | Verificação Merkle/Hashes obrigatória                            |
| v7    | Sistema → User (requer quórum)                                  | Bypass do quórum; acesso sem autorização           | Quórum obrigatório para decifração (k=3)                         |
| v8    | User → Quórum (solicita chaves)                                 | Solicitação fraudulenta de frações                 | Autenticação do solicitante; <br>auditoria                       |
| v9    | Quórum → Enclave (submissão Shamir)                             | Injeção de frações falsas                          | Validação das frações; <br>enclave isolado                       |
| v10   | Sistema → Enclave (envia evidência + chave pública cliente)     | Injeção de dados maliciosos no enclave             | Enclave recebe apenas dados necessários; <br>backend não decifra |
| v11   | Reconstituição + decifração + re-cifragem no enclave            | Exfiltração da EK ou dos dados em claro da RAM     | Zeroization (RNS04); <br>E2EE contra o backend                   |
| v12   | Enclave → Sistema (dados cifrados)                              | Interceptação do pacote cifrado                    | Cifrado com chave pública do cliente; <br>ilegível para backend  |
| v13   | Zeroization no enclave                                          | Falha na limpeza da RAM                            | Memory wipe forçado (RNS04);<br>verificação                      |
| v14   | Sistema → Gateway (pacote cifrado + notas pessoais)             | Fuga de notas pessoais                             | Notas pessoais cifradas com chave privada de identidade          |
| v15   | Gateway → User (entrega)                                        | Interceptação no último salto                      | [[WireGuard]]; <br>payload já cifrado E2EE                       |
| v16   | Descifração no cliente                                          | Comprometimento do cliente; chave privada exposta  | Chave privada em TPM/Secure Enclave; zeroization local           |
