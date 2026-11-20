> [!info] Definição
> **Adulteração de registos** (_tampering_) é a **alteração, inserção ou eliminação não autorizada** de dados ou de logs, normalmente para esconder rastos ou fazer passar informação falsa. No caso de provas forenses, é uma das ameaças mais graves: destrói a confiança na cadeia de custódia.

## Em linguagem simples

É como alguém que entra no livro de registo da escola e apaga uma falta, ou muda uma nota. O livro continua a parecer um livro normal — mas já não conta a história verdadeira.

## Como o ataque funciona

1. O atacante obtém acesso de escrita (BD, ficheiro, painel, backup).
2. Altera um valor, apaga uma linha ou troca um bloco por outro.
3. Se não houver deteção, a alteração passa despercebida.
4. Em logs, apaga precisamente o evento que o incrimina.

## Exemplo no BatEvidence

Trocar o hash de uma prova no ledger, mudar o conteúdo de uma nota antiga, ou apagar um evento `QUORUM_INIT` da cadeia de auditoria. Se nada detetasse a mudança, a prova deixaria de ser fidedigna.

## Porque é perigoso

- Quebra a **integridade** e a **cadeia de custódia**.
- Torna a prova inadmissível ou enganadora.
- Permite **esconder** acessos indevidos e responsabilidades.

## Como o BatEvidence se protege

- **WORM (RNS05):** após o fecho da gravação, `UPDATE` e `DELETE` são revogados.
- **Hash-chaining (RF12/RNS07):** cada bloco contém o hash do anterior; alterar um invalida todos os seguintes.
- **Árvore de Merkle (RNS06):** a raiz global muda se qualquer folha mudar.
- **Cadeia de auditoria assinada (RF14/RNS20):** cada evento é assinado pelo autor.
- **Assinatura [[Ed25519]]** dos blocos: impede reescrita silenciosa.

## Como testar

- **TC-RF12-01/02**, **TC-RNS05-01** e **TC-RNS20-01** em [[Intregavel 4]].
- Vetores **v6/v6a** em [[Ataques e Vetores]].

_Referências Relacionadas:_ [[Requesitos]], [[Intregavel 4]], [[Ataques e Vetores]], [[Falsificação de Assinatura]], [[Exfiltração de Dados e Chaves]], [[Bat (couf couf) Evidence A Wayne]]
