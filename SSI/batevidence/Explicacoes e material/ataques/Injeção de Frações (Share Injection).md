> [!info] Definição
> **Injeção de frações** é um ataque ao esquema de partilha de segredos (Shamir) em que o atacante **submete frações falsas ou corrompidas** durante a reconstrução do segredo. O objetivo pode ser impedir a abertura legítima, fazer o sistema reconstruir uma chave errada, ou obter informação sobre o segredo verdadeiro.

## Em linguagem simples

Imagina uma receita que só funciona se juntarem três ingredientes certos. O atacante mete um ingrediente falso na mesa. Se o cozinheiro não verificar, o bolo sai mal — ou sai uma coisa completamente diferente.

## Como o ataque funciona

1. O quórum vai reconstruir a chave do caso com $k=3$ frações.
2. Um participante malicioso (ou comprometido) envia uma fração **que não é a sua**.
3. Uma implementação ingénua combina as frações e obtém uma chave errada, falhando a decifra — ou pior, revela informação.
4. O atacante pode repetir com frações escolhidas para **afinar** o ataque.

## Exemplo no BatEvidence

Na sessão de quórum, o enclave recebe 3 frações (por exemplo, 2 do Alfred + 1 de um membro). Se não validar as frações antes de reconstruir, uma fração injetada pode bloquear a abertura ou comprometer a sessão. O esquema atual é $k=3, n=6$, com 2 frações no Root CA.

## Porque é perigoso

- **Negação de serviço** à abertura de provas legítimas.
- Pode levar à **reconstrução de uma chave errada** e a falhas difíceis de diagnosticar.
- Em implementações fracas, pode **revelar bits** do segredo verdadeiro.

## Como o BatEvidence se protege

- **Validação das frações** antes da reconstrução (RNS11), com rejeição e auditoria.
- **Compromissos de partilha (VSS)** ou valor de verificação da chave reconstruída.
- **Enclave isolado (RNS21):** recebe apenas o necessário e não expõe o segredo.
- **Duplo controlo no HSM (RNS15)** para as 2 frações do Alfred.
- **Auditoria do `QUORUM_INIT`** com autores e resultado, sem registar as frações (RF14).

## Como testar

- **TC-Q-06** em [[Intregavel 4]] — 2 frações válidas + 1 corrompida deve falhar e ser auditado.
- **TC-RF09-01** — reconstrução correta com 3 frações válidas.
- Vetor **v9** em [[Ataques e Vetores]].

_Referências Relacionadas:_ [[Requesitos]], [[Intregavel 4]], [[Ataques e Vetores]], [[Escalada de Privilégios]], [[Falsificação de Assinatura]], [[Bat (couf couf) Evidence A Wayne]]
