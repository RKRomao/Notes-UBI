##  Requisitos Funcionais (RF)
### Gestão de Casos e Ingestão de Provas

- RF01 — Criação e Gestão de Casos de Investigação:  
    O sistema deve permitir a criação, categorização e encerramento de casos de investigação criminal, servindo cada caso como entidade agregadora de evidências e notas forenses.

- RF02 — Ingestão de Prova Forense no Terreno:  
    O sistema deve permitir aos agentes operacionais (BATMAN-01, NIGHTWING-01, ROBIN-01) o depósito de artefactos digitais (imagens de disco, ficheiros binários, capturas .pcap), acompanhados de metadados contextuais (data/hora local, tipo de ficheiro, identificador do caso).

- RF03 — Referenciação Cruzada de Provas (Cross-Referencing):  
    O sistema deve suportar a associação de uma mesma evidência a múltiplos casos criminais através de apontadores lógicos, sem duplicar o ficheiro físico nem quebrar o seu identificador criptográfico único.

- RF04 — Submissão por Mailbox Pública Exterior (Public Drop-Box):  
    O sistema deve disponibilizar uma mailbox pública externa para submissão unidirecional (write-only) por parte do GCPD (GCPD-01), mantendo os ficheiros num buffer exterior isolado da rede privada e sem qualquer ligação à base de dados central.

- RF05 — Validação Manual e Admissão na Base de Dados:  
    O sistema deve exigir que analistas autorizados da Bat-Family (ORACLE-01) inspecionem os artefactos retidos na quarentena exterior e aprovem formalmente a sua entrada no perímetro interno e registo na base de dados.
###  Integridade, Pareceres e Quórum

- RF06 — Adição de Anotações Forenses Encadeadas:  
    O sistema deve permitir a peritos (ORACLE-01) anexar pareceres técnicos e notas a uma evidência, de maneira a preservar o ficheiro original em estado estritamente inalterado e encadeando a nova anotação à anterior por resumo criptográfico.

- RF07 — Consulta e Auditoria de Integridade a Pedido:  
    O sistema deve permitir verificar a qualquer momento se uma evidência arquivada se mantém idêntica ao momento da recolha, recalculando o seu hash e confrontando-o com a assinatura original e a raiz da árvore de Merkle.

- RF08 — Convocação de Sessão de Quórum:  
    O sistema deve disponibilizar um fluxo de votação e agregação de frações de chave secreta para autorizar a abertura de provas marcadas com nível de confidencialidade.

- RF09 — Reconstituição Temporária de Segredo em Memória:  
    O sistema deve reunir as frações submetidas e, caso atinjam o limiar mínimo ($k=3$), reconstituir a chave simétrica de decifração em memória RAM volátil, permitindo a visualização da prova pelo grupo durante a sessão.

- RF10 — Gestão do Ciclo de Vida de Certificados e Acessos:  
    O sistema deve permitir à Root CA (Alfred, HUM-ALFRED-00) emitir credenciais, listar certificados ativos, publicar a CRL e revogar imediatamente chaves e túneis VPN de dispositivos comprometidos.

- RF11 — Autenticação Mútua e Autorização por Caso:  
    O sistema deve autenticar todos os membros com certificados X.509 de folha [[Ed25519]] emitidos pela Root CA, sobre [[mTLS]] no perímetro interno e [[WireGuard]] no exterior, e autorizar cada operação de leitura ou escrita em função do papel e da atribuição ao caso (menor privilégio), negando por omissão.

- RF12 — Registo Encadeado do Caso (Case Ledger):  
    O sistema deve manter, por caso, uma sub-cadeia de blocos imutável — Bloco 0 (Genesis), blocos de prova/nota e bloco terminal — em que cada bloco contém o hash do anterior, o autor, o timestamp e a assinatura [[Ed25519]], impedindo a alteração, inserção ou remoção retroativa de registos.

- RF13 — Encerramento e Selagem Criptográfica do Caso:  
    O sistema deve permitir o encerramento formal de um caso, registando um bloco terminal com a flag "Encerrado" e aplicando um smart contract LOCK que rejeita novos depósitos e notas, deixando o caso apenas em leitura (*cold case*).

- RF14 — Cadeia de Auditoria Global (Access Sidechain):  
    O sistema deve registar numa sidechain de auditoria global todos os eventos de sistema — autenticações [[mTLS]], início e fecho de quórum, reconstruções no enclave, limpeza (`ENCLAVE_WIPE`), emissão/revogação de certificados e acessos ao WORM — com autor, timestamp, resultado e encadeamento por hash, sem nunca registar chaves privadas, frações ou segredos ECDH.

- RF15 — Consulta com Quórum e Entrega E2EE:  
    O sistema deve permitir a consulta de evidências e notas após validação do quórum, reconstruindo a chave do caso apenas dentro do Secure Enclave e re-cifrando o resultado para a chave pública efémera do pedido, de forma que o backend nunca aceda ao conteúdo em claro.

- RF16 — Separação e Filtragem de Notas Públicas e Privadas:  
    O sistema deve distinguir notas públicas (visíveis ao grupo autorizado) de notas privadas (cifradas para a chave de identidade do autor), filtrando as privadas no backend antes de qualquer entrega e mantendo ambas encadeadas à evidência e ao histórico.

- RF17 — Gestão do Ciclo de Vida das Chaves de Caso:  
    O sistema deve gerar, **dentro do enclave**, um par de chaves **X25519** de envoltório por caso (`PK_case` / `SK_case`). A chave pública `PK_case` é registada nos metadados do caso e comprometida no Bloco 0 (Genesis), sob assinatura [[Ed25519]]; o depositante recebe apenas `PK_case` e **nunca** vê `SK_case`. Cada prova é cifrada com uma **DEK** aleatória de 256 bits (AES-256-GCM); a DEK é envolta para `PK_case` (ECDH efémero → HKDF-SHA-256 → ChaCha20-Poly1305) e guardada apenas nessa forma envolta, acompanhada do `epk` e do nonce. `SK_case` **nunca é armazenada inteira**: é dividida em 6 frações de Shamir ($k=3$) — 1 por cada um dos quatro membros e 2 no HSM do Root CA — cada fração cifrada para o detentor (X25519 do seu certificado), com compromissos públicos de verificação (VSS). A chave é reconstituída exclusivamente dentro do enclave, em RAM, durante uma leitura autorizada, e zeroizada no fim. A rotação gera um novo par, re-envolve as DEKs no enclave mediante quórum e ancora um bloco `ROTACAO_CHAVE`, sem reescrever o texto cifrado no WORM. Não existe escrow nem cópia de segurança de `SK_case`: a perda de 4 ou mais frações torna o caso definitivamente ilegível (*crypto-shredding* por desenho).

- RF18 — Verificação Contínua de Revogação (CRL):  
    O sistema deve validar o estado de cada certificado contra a CRL em todas as operações autenticadas e rejeitar pedidos de certificados revogados, expirados ou fora da janela de validade.

- RF19 — Suporte a Provas em Ficheiro e em Registo Estruturado:  
    O sistema deve aceitar como evidência tanto ficheiros binários como registos estruturados, calculando o hash sobre a representação canónica e preservando o formato original para verificação futura.

- RF20 — Recibo de Depósito com Prova de Inclusão:  
    O sistema deve emitir, para cada depósito aceite, um recibo assinado com hash, timestamp e prova de inclusão na árvore de Merkle, permitindo ao depositante verificar a integridade sem aceder ao conteúdo.

- RF21 — Detonação em Sandbox antes da Quarentena:  
    Nenhum artefacto submetido pela drop-box pode ser gravado no buffer de quarentena sem passar por um pré-filtro estático (tipo real, limites de tamanho/compressão, canonicidade do URI) e por detonação dinâmica numa sandbox isolada. O veredicto (motores locais, regras YARA e comportamento) é assinado pelo serviço de análise e indexado pelo `SHA-256` dos bytes efetivamente armazenados, nunca pelo nome do ficheiro. Amostras marcadas como maliciosas ou **inconclusivas** ficam bloqueadas para admissão; a "ausência de deteção" não equivale a admissão automática. É proibido enviar provas para serviços externos de multi-scanner.

- RF22 — Estado do Depósito Independente do Veredicto:  
    O recibo de depósito (RF20) é emitido no momento da receção e do cálculo do hash, mesmo que a análise ainda esteja em curso, para que uma indisponibilidade da sandbox não impeça o depósito de prova. O veredicto condiciona apenas a admissão (RF05), que fica pendente até existir resultado assinado.

  
---

## Requisitos Não-Funcionais e de Segurança (RNS)

### Confidencialidade e Ocultação de Comunicações
- RNS01 — Ocultação e Cifragem de Rede (Stealth VPN):  
    Todo o tráfego entre terminais de campo e a central deve ser transportado através de túneis WireGuard (Noise Protocol Framework, Curve25519, ChaCha20-Poly1305). O servidor não deve responder a varrimentos de portas nem a pacotes não autenticados, mantendo-se invisível na Internet pública.

- RNS02 — Cifragem de Provas em Repouso:  
    Evidências classificadas como confidenciais devem ser cifradas em disco com AES-256-GCM, recorrendo a uma **DEK** aleatória de 256 bits gerada por prova. A DEK nunca é guardada em claro: é envolta para a chave pública do caso (`PK_case`, X25519 → HKDF-SHA-256 → ChaCha20-Poly1305) e armazenada apenas sob essa forma, junto do `epk` e do nonce.

- RNS03 — Eliminação Segura da Chave Privada do Caso (Zero-Knowledge at Rest):  
    A chave privada do caso (`SK_case`) **nunca existe em repouso**: existe apenas como $n=6$ frações de Shamir, cada uma cifrada para o detentor, e como material volátil dentro do enclave durante uma leitura autorizada. Qualquer reconstituição tem de ser zeroizada (RNS04) no fim da sessão. Não é permitido manter `SK_case` em disco, swap, *core dump* ou cache do enclave entre operações.

- RNS04 — Zeroização Forçada de Memória (Memory Zeroization):  
    Terminada a sessão de leitura de uma prova autorizada por quórum, o bloco de memória RAM onde residiu a chave reconstruída deve ser imediatamente sobrescrito com zeros (memory wipe).

### 2.2. Integridade e Imutabilidade

- RNS05 — Imutabilidade Física e Lógica (Volume WORM):  
    O armazenamento das evidências originais deve operar sob política WORM (Write Once, Read Many). Logo após o fecho da gravação inicial, os privilégios de escrita e eliminação devem ser permanentemente revogados a nível de sistema operativo e permissões de sistema de ficheiros.

- RNS06 — Integridade Contínua via Árvore de Merkle:  
    Cada nova evidência depositada deve ter o seu resumo SHA-256 inserido como nó folha na árvore de Merkle do repositório, sendo recalculada a raiz global (Merkle Root) para permitir provas criptográficas de inclusão e histórico imutável.

- RNS07 — Encadeamento Criptográfico de Anotações (Hash-Chaining):  
    As anotações periciais devem formar uma lista ligada criptográfica (mini-ledger), onde cada bloco $i$ contém obrigatoriamente o hash do bloco anterior ($H_{i-1}$), inviabilizando a alteração, substituição ou eliminação retroativa de notas sem quebra de integridade.


### 2.3. Autenticidade, Não-Repúdio e Gestão de Chaves

- RNS08 — Assinatura Digital e Não-Repúdio na Origem:  
    Todas as submissões de provas e anotações devem ser assinadas com [[Ed25519]] (algoritmo por omissão do perfil criptográfico), garantindo a autoria inequívoca e a impossibilidade de o autor repudiar o registo efetuado. O RSA-PSS (≥ 3072) é reservado para certificados de parceiros externos que o exijam e para a Root CA, nunca para provas/notas/blocos.

- RNS09 — Confirmação Institucional Bilateral de Entrega:  
    O servidor deve assinar digitalmente um comprovativo com carimbo temporal fidedigno para cada depósito válido, assegurando que nem o agente pode refutar o envio, nem a central pode refutar a receção íntegra.

- RNS10 — Proteção de Material Criptográfico em Hardware (TPM / Enclave):  
    As chaves privadas dos agentes de campo devem ser geradas e mantidas em hardware criptográfico dedicado (TPM 2.0 ou Secure Enclave móvel), impedindo a exportação ou exfiltração das chaves caso o sistema operativo do terminal seja comprometido.

- RNS11 — Esquema de Limiar Matemático de Shamir ($k=3, n=6$):  
    A partilha da **chave privada do caso** (`SK_case`, escalar X25519) deve garantir segurança teórica da informação (information-theoretic security): a posse de menos de 3 frações não pode conceder qualquer vantagem computacional sobre o segredo. As 6 frações distribuem-se por 1 fração a cada um dos quatro membros e **2 frações ao Root CA (Alfred)**, pelo que o limiar de 3 é atingido por **Alfred + 1 membro** ou por **3 membros** sem o Alfred; o Alfred, sozinho, fica sempre abaixo do limiar. Cada fração é acompanhada de um **compromisso público de verificação** (Feldman/VSS sobre um grupo de ordem prima, Ristretto255), sem o qual o esquema não deteta frações corrompidas; o enclave tem de validar cada fração contra os compromissos **antes** de reconstruir e abortar se alguma falhar.

- RNS13 — Envelope de Pedido Efémero com Forward Secrecy:  
    Cada pedido de consulta deve transportar um par de chaves X25519 gerado para esse pedido e nunca reutilizado, acompanhado de `request_id`, nonce de 128 bits, timestamp e TTL curto. O *transcript* do pedido (`request_id`, caso, nonce, timestamp, chave pública efémera) deve ser assinado com [[Ed25519]], ligando a chave efémera à identidade autenticada e impedindo a sua substituição pelo backend. A resposta é cifrada pelo enclave para a chave pública efémera (X25519 → HKDF-SHA-256 → ChaCha20-Poly1305); após a descifragem, o cliente deve fazer *zeroing* da chave privada efémera e do segredo ECDH (RNS04). O servidor guarda apenas `request_id` e o *hash* do nonce até ao fim do TTL, rejeitando pedidos expirados, fora da janela de relógio ou com nonce repetido.

### 2.4. Controlo de Acesso e Segregação de Parceiros

- RNS12 — Isolamento Estrito da Mailbox Pública e Menor Privilégio:  
    A mailbox pública de submissão do GCPD deve operar fora do perímetro da VPN WireGuard e sem acesso à base de dados. O parceiro externo não possui certificados de rede privada nem permissões de leitura, ficando os ficheiros restritos a um buffer exterior de quarentena até admissão manual por um elemento da Bat-Family. A admissão é feita por um gateway de quarentena separado, ao qual apenas Oracle (ORACLE-01) autentica com credencial de leitura restrita ao buffer exterior; a partir daí o ficheiro é re-validado e ancorado no perímetro interno.

### 2.5. Perímetro, Hardware e Operação

- RNS14 — mTLS Interno e Prevenção de Downgrade:  
    As comunicações entre serviços internos devem usar [[mTLS]] com certificados [[X.509]], e o verificador deve aceitar apenas os algoritmos da lista permitida ([[Ed25519]] para folhas, [[RSA|RSA-PSS]] para a Root CA), rejeitando qualquer negociação para algoritmos mais fracos.

- RNS15 — Duplo Controlo no HSM e Custódia das Frações do Root CA:  
    As 2 frações de Shamir detidas pelo Root CA devem residir num HSM offline e só podem ser libertadas com duplo controlo (presença física e autorização assinada do caso), ficando cada libertação registada na cadeia de auditoria; o compromisso do HSM mais um membro tem de ser detetável e auditável.

- RNS16 — Carimbo Temporal Fidedigno e Sincronização de Relógio:  
    O sistema deve usar uma fonte de tempo fidedigna e sincronizada para todos os timestamps de blocos, recibos e envelopes, rejeitando pedidos cuja janela de relógio exceda o limite definido (ex.: ±60 s).

- RNS17 — Redundância e Recuperação:  
    O sistema deve manter servidor principal, backup a quente e backup a frio, com verificação periódica de integridade dos backups e procedimento de recuperação testado que preserve a cadeia de custódia e os hashes originais.

- RNS18 — Segregação de Redes e Firewalls:  
    O perímetro deve ser segmentado por firewalls físicas entre o router de acesso e os serviços internos, com regras de acesso combinadas com a autenticação [[mTLS]]/[[WireGuard]], impedindo que um externo alcance a base de dados ou o WORM.

- RNS19 — Menor Privilégio e Segregação de Funções:  
    Cada identidade deve dispor apenas das permissões necessárias ao seu papel, com segregação entre quem deposita, quem valida/anota, quem administra a PKI e quem custodia frações; nenhuma identidade isolada pode alterar ou eliminar provas já seladas.

- RNS20 — Registo de Auditoria de Todas as Operações C/R/D:  
    Todas as operações de criação, leitura e eliminação/revogação devem gerar um evento assinado na cadeia de auditoria global, com identificação do autor, alvo e resultado, garantindo rastreabilidade e não-repúdio.

- RNS21 — Isolamento do Secure Enclave:  
    O Secure Enclave deve executar apenas o código estritamente necessário à reconstituição e re-cifragem, receber o mínimo de dados possível, não expor chaves ao backend e limpar a RAM após cada operação (RNS04).

- RNS22 — Desempenho em Liga Satélite:  
    O transporte [[WireGuard]] sobre ligações satélite deve ser configurado com MTU 1280–1360, MSS 1200–1300, BBR v2, buffers adequados e keepalive 20–30 s, e o sistema deve degradar de forma controlada em ligações de alta latência (GEO) ou L-band.

- RNS23 — Isolamento da Sandbox de Análise:  
    A sandbox deve correr em microVM com kernel próprio (sem kernel partilhado com o host nem com o serviço de quarentena), arrancar de imagem dourada imutável, ser destruída e recriada a cada amostra, não receber credenciais nem acesso a chaves, e ter *egress* bloqueado por omissão — a única saída permitida é um *sinkhole* local que simula Internet. O host de análise reside em VLAN própria, sem rotas para a BD, o WORM ou o backend, e o hipervisor é atualizado com o mesmo rigor dos sistemas expostos. Amostras nunca são enviadas para serviços externos.

- RNS24 — Disponibilidade e Anti-Abuso da Drop-Box:  
    O gateway de submissão deve impor quota por identidade e por caso, limites de tamanho e de taxa, lista de tipos permitidos e rejeição *fail-closed* acima dos limites, com registo auditado de cada submissão recusada. A fila de análise tem profundidade máxima, tempo de detonação limitado e pressão de retorno (*backpressure*), de modo que uma campanha de *flooding* degrade a triagem mas não impeça a emissão de recibos (RF22); a admissão permanece bloqueada enquanto não existir veredicto.

    

  
