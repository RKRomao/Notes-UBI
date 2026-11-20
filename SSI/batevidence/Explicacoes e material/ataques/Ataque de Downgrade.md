> [!info] Definição
> Um **ataque de downgrade** ("ataque de rebaixamento") consiste em **forçar duas partes a usar uma versão mais fraca** de um protocolo ou algoritmo do que aquela que conseguem usar. Em vez de quebrar a cifra forte, o atacante convence o sistema a usar uma cifra fraca ou nenhuma.

## Em linguagem simples

Imagina que o banco sabe usar tinta indelével, mas o atacante consegue convencer o balcão a usar **lápis**. A partir daí, apaga e reescreve o que quiser. O problema não é o cofre: é terem aceitado uma proteção inferior.

## Como o ataque funciona

1. O cliente e o servidor anunciam as versões/algoritmos que suportam.
2. O atacante **remove ou corrompe** as opções fortes no meio da negociação.
3. O servidor, mal configurado, aceita a opção fraca que sobrou.
4. A comunicação continua a funcionar — mas sem as garantias esperadas.

## Exemplo no BatEvidence

Entre serviços internos, um atacante poderia tentar negociar TLS antigo, SHA-1 ou RSA-1024 em vez de [[Ed25519]]/RSA-4096. Se o verificador aceitasse "qualquer algoritmo", a autenticação passaria a ser muito mais fácil de falsificar.

## Porque é perigoso

- Permite **falsificação de assinaturas** e **quebra de cifra** com esforço menor.
- Costuma ser **silencioso**: o sistema funciona, só que mal.
- Serve de porta de entrada para MitM e substituição de chave.

## Como o BatEvidence se protege

- **Lista de algoritmos permitidos:** o verificador aceita apenas [[Ed25519]] nas folhas e [[RSA|RSA-PSS]] na Root CA, rejeitando o resto (RNS14).
- **Sem fallback silencioso:** se a negociação falhar, o pedido é rejeitado (*fail-closed*), não rebaixado.
- **Perfil criptográfico fixo:** o sistema define um único perfil, para que "Ed25519 ou RSA" nunca signifique "qualquer um".
- **Auditoria** de tentativas de negociação inválidas (RF14).

## Como testar

- **TC-RNS14-02** em [[Intregavel 4]] — forçar SHA-1/RSA-1024 e confirmar rejeição.
- Vetores **v5/v7** em [[Ataques e Vetores]].

_Referências Relacionadas:_ [[Requesitos]], [[Intregavel 4]], [[Ataques e Vetores]], [[Man-in-the-Middle (MitM)]], [[Falsificação de Assinatura]], [[mTLS]], [[Ed25519]], [[RSA]]
