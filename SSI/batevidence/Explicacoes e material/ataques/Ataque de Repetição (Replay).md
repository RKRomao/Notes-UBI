> [!info] Definição
> Um **ataque de repetição** (_replay attack_) acontece quando um atacante **captura uma mensagem válida** e a **reenvia mais tarde**, tal e qual, para produzir o mesmo efeito outra vez. Não é preciso quebrar criptografia: a mensagem original já estava correta e assinada.

## Em linguagem simples

Imagina que gravas a voz do teu chefe a dizer "autorizo o pagamento de 100 €" e voltas a reproduzir a gravação no dia seguinte. A frase é autêntica, mas o contexto já não é. O sistema ouve a voz certa e paga outra vez.

## Como o ataque funciona

1. O atacante interceta um pedido legítimo (ex.: um envelope de consulta).
2. Guarda-o sem alterar nada.
3. Reenvia-o para o servidor, às vezes horas ou dias depois.
4. Se o servidor não souber distinguir "pedido novo" de "pedido repetido", executa a operação outra vez.

## Exemplo no BatEvidence

Um pedido de consulta viaja num **envelope efémero** com `request_id`, nonce e TTL. Se o backend aceitasse o mesmo envelope duas vezes, um atacante que o capturasse poderia obter uma **segunda resposta** com a prova decifrada, ou duplicar um depósito. O envelope existe precisamente para que cada pedido valha **uma única vez**.

## Porque é perigoso

- Duplica operações (depósitos, transferências, acessos).
- Permite obter respostas que deviam ser de uso único.
- Pode servir para **correlacionar** ou **reutilizar** sessões antigas.

## Como o BatEvidence se protege

- **Nonce de 128 bits** por pedido, com cache do seu hash até ao fim do TTL (RNS13).
- **`request_id` de uso único:** um segundo pacote com o mesmo ID é rejeitado, mesmo dentro do TTL.
- **TTL curto** (ex.: 120 s) e **janela de relógio** (ex.: ±60 s): pedidos velhos ou fora de hora são recusados (RNS16).
- **Assinatura do *transcript*** com [[Ed25519]], ligando o pedido à identidade e à chave efémera.
- **Registo na auditoria** de pedidos aceites e rejeitados por repetição (RF14).

## Como testar

- **TC-RNS13-02** e **TC-RNS16-01** em [[Intregavel 4]] — reenviar o mesmo `request_id`/nonce e confirmar rejeição.
- **TC-Q-09** — replay da submissão de frações de quórum.
- Vetor **v1b** em [[Ataques e Vetores]].

_Referências Relacionadas:_ [[Requesitos]], [[Intregavel 4]], [[Ataques e Vetores]], [[Substituição de Chave (Key Substitution)]], [[Man-in-the-Middle (MitM)]], [[Bat (couf couf) Evidence A Wayne]]
