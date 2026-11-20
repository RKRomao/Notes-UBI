> [!info] Definição
> **Escalada de privilégios** é quando um utilizador ou processo consegue **mais permissões do que aquelas que lhe foram atribuídas**. Pode ser *vertical* (passar de utilizador normal a administrador) ou *horizontal* (aceder aos recursos de outro utilizador com o mesmo nível).

## Em linguagem simples

É como um funcionário que tem cartão para a sua sala, mas descobre forma de usar o cartão do chefe para abrir o cofre. Não lhe deram essa permissão — ele **arranjou maneira de a obter**.

## Como o ataque funciona

1. O atacante explora uma verificação em falta ou mal feita (ex.: o servidor confia num campo enviado pelo cliente).
2. Altera o seu papel, o ID de outro utilizador ou um *token*.
3. O sistema aceita a alteração e concede acesso indevido.
4. A partir daí, pode ler, alterar ou apagar o que não devia.

## Exemplo no BatEvidence

O Nightwing tenta validar e anotar provas como se fosse a Oracle, ou tenta executar uma operação de `DELETE` que só o Root CA pode fazer na gestão do repositório. Se as permissões fossem verificadas no cliente em vez de no servidor, bastava alterar o pedido.

## Porque é perigoso

- Quebra o **controlo de acesso** e a **segregação de funções**.
- Pode levar a **adulteração** ou **eliminação** de provas.
- Muitas vezes combina-se com **IDOR** e **movimento lateral**.

## Como o BatEvidence se protege

- **Autorização no servidor**, nunca no cliente (RF11).
- **RBAC/ABAC por papel e por caso**, com negação por omissão (RNS19).
- **Segregação de funções:** quem deposita não valida; quem emite identidades não deposita provas.
- **Certificados [[X.509]]** ligam a identidade ao papel; a CRL revoga acessos (RF10/RF18).
- **Auditoria assinada** de todas as operações C/R/D (RNS20).

## Como testar

- **TC-RF11-03**, **TC-RNS19-01** e **TC-RNS19-02** em [[Intregavel 4]].
- Vetores **v4/v8** em [[Ataques e Vetores]].

_Referências Relacionadas:_ [[Requesitos]], [[Intregavel 4]], [[Ataques e Vetores]], [[IDOR]], [[Movimento Lateral]], [[Adulteração de Registos (Tampering)]], [[X.509]]
