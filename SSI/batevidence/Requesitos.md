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
    O sistema deve permitir à autoridade central (AGENT-00) emitir credenciais, listar certificados ativos e revogar imediatamente chaves e túneis VPN de dispositivos comprometidos.

  
---

## Requisitos Não-Funcionais e de Segurança (RNS)

### Confidencialidade e Ocultação de Comunicações
- RNS01 — Ocultação e Cifragem de Rede (Stealth VPN):  
    Todo o tráfego entre terminais de campo e a central deve ser transportado através de túneis WireGuard (Noise Protocol Framework, Curve25519, ChaCha20-Poly1305). O servidor não deve responder a varrimentos de portas nem a pacotes não autenticados, mantendo-se invisível na Internet pública.

- RNS02 — Cifragem de Provas em Repouso:  
    Evidências classificadas como confidenciais devem ser cifradas em disco com AES-256-GCM, recorrendo a uma chave simétrica efémera de evidência (EK).

- RNS03 — Eliminação Segura da Chave Mestre (Zero-Knowledge at Rest):  
    Após a distribuição das $n=5$ frações de Shamir, a chave simétrica EK deve ser destruída da memória. A chave de descifragem não pode residir em repouso em nenhum disco ou suporte permanente.

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
    Todas as submissões de provas e anotações devem ser assinadas com chaves assimétricas RSA-PSS-2048, garantindo a autoria inequívoca e a impossibilidade de o autor repudiar o registo efetuado.

- RNS09 — Confirmação Institucional Bilateral de Entrega:  
    O servidor deve assinar digitalmente um comprovativo com carimbo temporal fidedigno para cada depósito válido, assegurando que nem o agente pode refutar o envio, nem a central pode refutar a receção íntegra.

- RNS10 — Proteção de Material Criptográfico em Hardware (TPM / Enclave):  
    As chaves privadas dos agentes de campo devem ser geradas e mantidas em hardware criptográfico dedicado (TPM 2.0 ou Secure Enclave móvel), impedindo a exportação ou exfiltração das chaves caso o sistema operativo do terminal seja comprometido.

- RNS11 — Esquema de Limiar Matemático de Shamir ($k=3, n=5$):  
    A partilha de segredos de provas críticas deve garantir segurança teórica da informação (information-theoretic security): a posse de menos de 3 frações não pode conceder qualquer vantagem computacional sobre o segredo.

### 2.4. Controlo de Acesso e Segregação de Parceiros

- RNS12 — Isolamento Estrito da Mailbox Pública e Menor Privilégio:  
    A mailbox pública de submissão do GCPD deve operar fora do perímetro da VPN WireGuard e sem acesso à base de dados. O parceiro externo não possui certificados de rede privada nem permissões de leitura, ficando os ficheiros restritos a um buffer exterior de quarentena até admissão manual por um elemento da Bat-Family.
    

  
**