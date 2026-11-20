> [!info] Definição
> Um ataque de **substituição de chave** (_key substitution_) acontece quando o atacante **troca a chave pública** de uma das partes pela sua própria, no caminho. A vítima pensa que está a cifrar para a pessoa certa, mas está a cifrar para o atacante — ou a aceitar uma assinatura que não é de quem julga.

## Em linguagem simples

Imagina que trocas o cadeado do teu cacifo pelo cadeado do atacante. Tu fechas o cacifo com o cadeado dele, convencido de que é o teu; ele abre-o quando quiser. A chave que usaste estava certa para o cadeado errado.

## Como o ataque funciona

1. O cliente envia a sua **chave pública efémera** para receber uma resposta cifrada.
2. O atacante (ou um backend malicioso) **substitui** essa chave pela chave pública dele.
3. O servidor cifra a resposta para a chave do atacante.
4. O atacante decifra a resposta e o cliente nunca recebe nada — ou recebe uma versão alterada.

## Exemplo no BatEvidence

No [[Bat (couf couf) Evidence A Wayne#Pedidos Efémeros (Ephemeral Request Envelope)|Envelope de Pedido Efémero]], o cliente envia `pk_efémera`. Se essa chave não estivesse ligada à identidade autenticada, o backend poderia trocá-la e ler a prova que o enclave devolve. Por isso o *transcript* (`request_id`, caso, nonce, timestamp, `pk_efémera`) é **assinado com [[Ed25519]]**.

## Porque é perigoso

- Quebra a **confidencialidade** sem tocar na cifra.
- Permite **impersonação** e respostas falsas.
- É especialmente grave quando o próprio servidor é o adversário (modelo *zero-knowledge*).

## Como o BatEvidence se protege

- **Assinatura do *transcript*** com a chave de identidade [[Ed25519]]: substituir a chave invalida a assinatura (RNS13).
- **Certificados [[X.509]]** ligam a chave pública à identidade; a cadeia tem de validar até à Root CA (RF11).
- **Envelope E2EE:** o enclave faz ECDH com a chave assinada, não com uma chave "solta".
- **CRL:** uma chave de identidade revogada deixa de ser aceite (RF18).

## Como testar

- **TC-RNS13-01** em [[Intregavel 4]] — tentar substituir `pk_efémera` e confirmar rejeição.
- Vetor **v1b** em [[Ataques e Vetores]].

_Referências Relacionadas:_ [[Requesitos]], [[Intregavel 4]], [[Ataques e Vetores]], [[Man-in-the-Middle (MitM)]], [[Ataque de Repetição (Replay)]], [[X.509]], [[Ed25519]], [[X25519]]
