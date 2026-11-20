> [!info] Definição
> **Falsificação de assinatura** é tentar fazer passar uma mensagem como se tivesse sido assinada por outra pessoa, **sem ter a chave privada** dessa pessoa. Também inclui alterar o conteúdo depois de assinado, ou explorar erros na forma como a assinatura é verificada.

## Em linguagem simples

É como imitar a assinatura de alguém num documento. Se a imitação for boa, ou se ninguém comparar com a assinatura verdadeira, o documento parece legítimo. A criptografia resolve isto se a verificação for feita **sempre** e da forma correta.

## Como o ataque funciona

1. O atacante escolhe um alvo (ex.: Nightwing) e tenta obter a chave privada, ou explora um algoritmo fraco.
2. Constrói uma mensagem (ex.: "depositei a prova X") e assina-a com a chave que controla.
3. Se o servidor não verificar a assinatura contra o certificado do autor, aceita o depósito.
4. Outra variante: assinar **parte** dos dados e alterar o resto, ou verificar a assinatura sobre dados diferentes dos usados.

## Exemplo no BatEvidence

Um atacante tenta criar um bloco de prova assinado "por Nightwing" para introduzir uma evidência falsa na cadeia do caso. Se a assinatura [[Ed25519]] fosse verificada contra o certificado correto e sobre o *transcript* completo, o bloco seria rejeitado.

## Porque é perigoso

- Destrói o **não-repúdio**: deixa de se saber quem fez o quê.
- Permite **plantar provas** ou **negar autoria**.
- Compromete toda a cadeia de custódia.

## Como o BatEvidence se protege

- **[[Ed25519]] em provas, notas e blocos** (RNS08), com chaves de 32 bytes e assinaturas de 64 bytes.
- **Verificação contra o certificado do autor** e a cadeia [[X.509]] (RF11).
- **Assinatura sobre o *transcript* completo** (RNS13), não sobre um fragmento.
- **Chaves privadas em TPM/Secure Enclave** (RNS10), não exportáveis.
- **CRL** para bloquear chaves comprometidas (RF18).

## Como testar

- **TC-RNS08-01** em [[Intregavel 4]] — assinatura forjada e assinatura sobre dados alterados devem falhar.
- Vetor **v10** em [[Ataques e Vetores]].

_Referências Relacionadas:_ [[Requesitos]], [[Intregavel 4]], [[Ataques e Vetores]], [[Substituição de Chave (Key Substitution)]], [[Ataque de Downgrade]], [[Ed25519]], [[X.509]]
