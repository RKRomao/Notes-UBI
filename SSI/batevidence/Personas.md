### Alfred Pennyworth - Raiz de Confiança (Root of Trust)

![Alfred Pennyworth](https://upload.wikimedia.org/wikipedia/en/5/5c/Alfred_Pennyworth_%28Alex_Ross%29.jpg)

#### Personalization
Alfred Pennyworth, identificado como HUM-ALFRED-00, tem cerca de 70 anos e é o mordomo de longa data da família Wayne. Discreto, metódico e extremamente leal, vive na Batcaverna e conhece os segredos de todos os membros da família. A sua principal motivação é garantir que nada comprometa a segurança daqueles que serve.

#### Job-related
Administrador da Autoridade de Certificação (Root CA), Gestor da PKI Interna e Guardião de Custódia do Esquema de Segredos de Shamir (Escrow de Quórum). O seu trabalho consiste em emitir, renovar e revogar certificados [[X.509]]/[[Ed25519]]/[[RSA|RSA-PSS]], operar o HSM físico e iniciar sessões de reconstituição de segredos em emergências auditadas.

#### Education
Formação em engenharia de sistemas e criptografia aplicada, com vasta experiência em operação de PKI, gestão de HSM e protocolos de recuperação de emergência. Possui elevado nível técnico e é o único elemento com visão transversal de todos os agentes e dispositivos.

#### Relevance
Precisa do BatEvidence para assegurar a integridade de todo o ecossistema criptográfico. Quer poder gerir certificados, manter a CRL atualizada e garantir que nenhuma evidência é acedida sem o quórum exigido.

---

### Bruce Wayne (Batman) - Líder Operacional e Decisor

![Batman](https://upload.wikimedia.org/wikipedia/en/c/c7/Batman_Infobox.jpg)

#### Personalization
Bruce Wayne, identificado como BATMAN-01 (certificado BAT-CLI-001), é o líder da Bat(couf couf)Family. Herdeiro da fortuna Wayne, dedica a vida a proteger Gotham e reúne à sua volta um núcleo de colaboradores de confiança. É reservado, estratégico e assume a responsabilidade máxima pelas decisões da equipa.

#### Job-related
Empresário e filantropo na Wayne Enterprises, com atividade paralela de combate ao crime. No sistema, submete provas apreendidas em operações de alto escalão e convoca sessões de quórum para abertura de provas confidenciais. Detém 1 fração do segredo de Shamir (x₁, y₁).

#### Education
Especialista em investigação criminal, artes marciais e gestão de operações de alto risco. Possui formação avançada em análise forense e compreende os requisitos legais de cadeia de custódia.

#### Relevance
Quer garantir que as provas recolhidas em operações de alto risco sejam preservadas de forma irrefutável e que apenas o quórum autorizado possa aceder às evidências confidenciais.

---

### Dick Grayson (Nightwing) - Operador Tático no Terreno

![Nightwing](https://upload.wikimedia.org/wikipedia/en/8/8b/Nightwing_Vol_4_137_%282026_Cover_B_David_Nakayama_art.png)

#### Personalization
Dick Grayson, identificado como NIGHTWING-01 (certificado BAT-CLI-002), é um antigo protegido de Bruce Wayne e o primeiro Robin. Cresceu dentro da família Wayne e tornou-se um operador autónomo de confiança, conhecido pela agilidade e pela capacidade de atuar em ambientes hostis.

#### Job-related
Operador tático de campo e investigador independente. Atua frequentemente em zonas urbanas hostis (portos, armazéns clandestinos) e é responsável pela extração e depósito rápido de ficheiros suspeitos, pela assinatura digital local imediata e pela custódia de 1 fração do segredo de Shamir (x₂, y₂).

#### Education
Formação em acrobacia, combate e infiltração, complementada por treino técnico em captura de tráfego de rede e preservação de artefactos digitais. Nível técnico intermédio-alto, com foco em operações de campo.

#### Relevance
Precisa de depositar evidências de forma rápida e segura, mesmo com redes degradadas, e de garantir que a assinatura é feita no momento da apreensão. Quer minimizar o risco de perda de pacotes e de destruição do terminal.

---

### Tim Drake (Robin) - Especialista em Extração Digital de Campo

![Robin](https://upload.wikimedia.org/wikipedia/en/7/7f/Robin_%28Tim_Drake%29.png)

#### Personalization
Tim Drake, identificado como ROBIN-01 (certificado BAT-CLI-004), é o terceiro Robin e o mais técnico dos operadores de campo. Destaca-se pela atenção ao detalhe e pela capacidade de automatizar processos forenses complexos.

#### Job-related
Analista forense digital e operador de campo. Realiza clonagem forense local de computadores e servidores, executa rotinas automatizadas de hashing, gera manifestos de integridade e valida hashes SHA-256/512 e/ou BLAKE3 antes da submissão via [[mTLS]]. Detém 1 fração do segredo de Shamir (x₄, y₄).

#### Education
Formação em informática forense, com domínio de ferramentas de imagem de disco, hashing criptográfico e protocolos de comunicação segura ([[mTLS]]). Elevado nível técnico.

#### Relevance
Quer velocidade e automatismo na submissão: recolher a imagem forense, gerar o manifesto e obter o recibo assinado do servidor com o mínimo de intervenção manual, evitando interceções por redes falsas ou ataques MitM.

---

### Barbara Gordon (Oracle) - Guardiã Forense e Analista

![Barbara Gordon](https://upload.wikimedia.org/wikipedia/en/6/6c/Barbara_Gordon.jpg)

#### Personalization
Barbara Gordon, identificada como ORACLE-01 (certificado BAT-CLI-003), é filha do Comissário Gordon e antiga Batgirl. Após um incidente que a afastou do combate físico, reinventou-se como Oracle, tornando-se a mente analítica da equipa.

#### Job-related
Analista forense sénior e especialista em segurança da informação. Valida recibos de custódia, verifica resumos na árvore de Merkle, redige e assina pareceres periciais na cadeia de anotações e faz triagem de provas em quarentena. Detém 1 fração do segredo de Shamir (x₃, y₃).

#### Education
Formação em ciências forenses, criptografia e análise de dados, com experiência em cadeia de custódia e elaboração de pareceres periciais para tribunal. Elevado nível técnico e analítico.

#### Relevance
Quer assegurar que nenhum perito externo ou autoridade judiciária consiga contestar a validade das provas em tribunal, e teme que anotações anteriores sejam adulteradas para incriminar falsos suspeitos.

---

### Comissário James Gordon - Parceiro Institucional Externo (External Drop-Box)

![Comissário Gordon](https://upload.wikimedia.org/wikipedia/en/5/56/James_Gordon_%28DC_Comics_character%29.png)

#### Personalization
James Gordon, identificado como GCPD-01 (certificado GCPD-CLI-001), é o Comissário da Polícia de Gotham e aliado de longa data do Batman. Íntegro e experiente, representa a ponte entre a justiça formal e a atuação da Bat(couf couf)Family.

#### Job-related
Comissário da Polícia de Gotham (GCPD). No sistema, faz submissão cega (write-only blind drop) de artefactos recolhidos pela polícia, que ficam em área isolada de quarentena para triagem posterior pela Oracle, e recebe recibos de entrega com carimbo temporal e hash sem acesso ao conteúdo processado.

#### Education
Décadas de serviço policial, com profundo conhecimento dos procedimentos legais de recolha e preservação de prova e das limitações dos sistemas institucionais. Nível técnico básico-intermédio.

#### Relevance
Quer partilhar evidências forenses com a equipa do Batman de forma segura, mantendo o isolamento estrito entre os sistemas do GCPD e a Batcaverna, sem acesso de leitura ao repositório principal.

