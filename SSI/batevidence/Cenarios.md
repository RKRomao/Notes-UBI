### Cenário 1 - Recolha e Depósito de Prova no Terreno

Durante uma operação de campo, um agente da Bat(couf couf)Family apreende um artefacto digital no local do crime e precisa de o depositar no BatEvidence de forma segura. No terreno, a rede é instável e o tempo é limitado, pelo que o agente clona o artefacto, calcula o hash SHA-256 e assina digitalmente o resultado com a sua chave privada antes de submeter tudo por [[mTLS]] para o repositório. O sistema devolve um recibo assinado que atesta a integridade e a autoria do depósito, permitindo que a evidência seja posteriormente analisada sem que a sua cadeia de custódia seja posta em causa.

#### Nome do Cenário
Recolha e depósito de prova forense durante uma operação de campo.

#### Objetivo Geral
Recolher uma evidência digital no local do crime e depositá-la no BatEvidence com garantias de integridade, autoria e não-repúdio, preservando a cadeia de custódia.

#### O Que Está Envolvido para Atingir o Objetivo
Deslocação ao local, apreensão do artefacto, clonagem forense, cálculo de hash (SHA-256), assinatura digital local com a chave privada do agente ([[Ed25519]]) e submissão via [[mTLS]] para o repositório.

#### Personas dos Atores Envolvidos
Dick Grayson (Nightwing) e Tim Drake (Robin), com validação posterior por Barbara Gordon (Oracle).

#### Problema que o Sistema Existente Não Consegue Resolver
Os sistemas tradicionais não garantem a autenticidade e integridade das provas recolhidas no terreno nem registam de forma inequívoca a identidade do agente que as depositou, comprometendo a cadeia de custódia.

#### Possíveis Formas de Abordar o Problema
Assinatura digital no momento da apreensão, hashing imediato, comunicação segura por [[mTLS]] e registo imutável do recibo de depósito.

---

### Cenário 2 - Abertura Condicionada de Prova Confidencial

Uma prova particularmente sensível foi armazenada de forma que o seu conteúdo só possa ser revelado quando um número mínimo de membros da família estiver presente. Bruce Wayne convoca a sessão e os membros entregam as suas frações do segredo de Shamir. O limiar de 3 frações pode ser atingido de duas formas: **Alfred (que detém 2 frações no HSM) mais 1 membro**, ou **3 membros sem o Alfred**. Alfred, a partir da Batcaverna, reúne as frações no HSM e desbloqueia a evidência apenas depois de confirmado o limiar exigido. Toda a operação fica registada para auditoria, garantindo que nenhum elemento isolado conseguiria aceder à prova por sua conta.

#### Nome do Cenário
Abertura de uma prova confidencial através de reconstrução de segredo (quórum de Shamir).

#### Objetivo Geral
Permitir o acesso ao conteúdo de uma evidência apenas quando se reunir o número mínimo predefinido de membros da família, sem que nenhum elemento isolado — incluindo o Alfred com apenas 2 frações — consiga decifrá-la.

#### O Que Está Envolvido para Atingir o Objetivo
Convocação da sessão por Bruce Wayne, reunificação de 3 frações do segredo de Shamir (x₁…x₄ dos membros, ou x₅/x₆ do Alfred + 1 membro), inicialização da sessão pelo Alfred no HSM e desbloqueio auditado da evidência.

#### Personas dos Atores Envolvidos
Bruce Wayne (Batman), Alfred Pennyworth, Dick Grayson, Tim Drake e Barbara Gordon.

#### Problema que o Sistema Existente Não Consegue Resolver
Os sistemas convencionais de controlo de acesso baseiam-se num único administrador ou credencial, criando um ponto único de falha e de abuso de privilégios.

#### Possíveis Formas de Abordar o Problema
Esquema de partilha de segredos de Shamir, com limiar de membros, sessões auditadas e registo de cada reconstituição.

---

### Cenário 3 - Análise e Anotação de Provas

Depois de a prova estar depositada, Barbara Gordon consulta-a a partir do posto forense central, valida o recibo de custódia e verifica os resumos na árvore de Merkle. À medida que produz o parecer pericial, acrescenta anotações que ficam encadeadas criptograficamente e assinadas, sem nunca alterar a evidência original. Assim, o histórico de análise mantém-se rastreável e qualquer tentativa de adulteração de anotações anteriores torna-se detetável.

#### Nome do Cenário
Análise forense e adição de anotações a uma prova já depositada.

#### Objetivo Geral
Permitir que os analistas consultem as evidências e acrescentem pareceres e anotações, mantendo a evidência original intacta e o histórico rastreável.

#### O Que Está Envolvido para Atingir o Objetivo
Validação dos recibos de custódia, verificação dos resumos na árvore de Merkle, redação e assinatura digital de pareceres na cadeia de anotações (hash-chaining).

#### Personas dos Atores Envolvidos
Barbara Gordon (Oracle) e Bruce Wayne (Batman).

#### Problema que o Sistema Existente Não Consegue Resolver
A alteração ou sobreposição de anotações em sistemas tradicionais pode adulterar o histórico e pôr em causa a validade da prova em tribunal.

#### Possíveis Formas de Abordar o Problema
Separação física entre evidência original (WORM) e anotações, encadeamento criptográfico das anotações e assinatura digital de cada parecer.

---

### Cenário 4 - Submissão Externa pela Polícia de Gotham

O Comissário Gordon precisa de partilhar artefatos recolhidos pela polícia de Gotham para análise conjunta, sem expor os sistemas do GCPD nem obter acesso ao repositório principal. Através de um canal write-only, deposita os ficheiros numa área de quarentena isolada e recebe um recibo com carimbo temporal e hash, mas sem qualquer acesso ao conteúdo processado. A Oracle faz depois a triagem das provas em quarentena, mantendo o isolamento entre as duas instituições.

#### Nome do Cenário
Depósito cego de evidências pela Polícia de Gotham (GCPD).

#### Objetivo Geral
Permitir que o Comissário Gordon submeta artefactos recolhidos pela polícia para análise conjunta, sem acesso ao repositório principal.

#### O Que Está Envolvido para Atingir o Objetivo
Submissão write-only para uma área de quarentena isolada, receção de recibo com carimbo temporal e hash, e triagem posterior pela Oracle.

#### Personas dos Atores Envolvidos
Comissário James Gordon e Barbara Gordon (Oracle).

#### Problema que o Sistema Existente Não Consegue Resolver
A partilha de provas entre instituições exige isolamento estrito entre sistemas, algo que os repositórios partilhados tradicionais não asseguram.

#### Possíveis Formas de Abordar o Problema
Drop-box write-only, área de quarentena dedicada e princípio do menor privilégio.

---

## Arquitetura

A arquitetura do sistema é simples: cada utilizador é detentor de uma chave de acesso, que dá acesso à app. Depois de autenticados, os utilizadores devem ter acesso às provas que necessitarem para os casos a que têm acesso e às notas relativas a esse caso. Os casos podem partilhar provas (*cross reference*).
