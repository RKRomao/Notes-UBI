---
epoch: 1795125028468
mode: agent
backendId: opencode
sessionId: "ses_e0a3667e4ffesp9CAKqGUd6x8R"
tags:
  - copilot-conversation
agentLabel: "Expandir pedidos efêmeros no documento"
usage: '{"usedTokens":49109,"contextWindow":200000,"updatedAt":1795126354876}'
---

**user**: espandir a ideia dos pedidos efemeros e colocala nos devidos logares no docomento
[Timestamp: 2026/11/19 21:50:28]

**ai**: Expandi a ideia dos **pedidos efémeros** e distribuí-a pelos pontos do documento onde ela é efetivamente usada, em vez de a deixar como uma nota solta.

## O que foi acrescentado em [[Bat (couf couf) Evidence A Wayne]]

1. **Perfil Criptográfico** — duas linhas novas: `X25519` para as chaves de sessão efémeras (um par por pedido, *forward secrecy*) e `HKDF-SHA-256 → ChaCha20-Poly1305` para a derivação do envelope de resposta.

2. **Acesso Seguro a Evidências** — a Fase 1 deixou de dizer apenas "gera um par de chaves e envia"; agora descreve o envelope completo (`request_id`, nonce, timestamp, TTL, ID do caso, chave pública efémera) e a assinatura [[Ed25519]] do *transcript*, com ligação para a nova secção. As fases 3 e 4 passaram a falar da chave **efémera** (e não da "chave pública do cliente") e do *zeroing* final.

3. **Diagrama de sequência da consulta** — passou a mostrar a geração do par de uso único, a assinatura do envelope, a validação de CRL/TTL/nonce no backend, a re-cifragem pelo enclave com o seu próprio par efémero e o *zeroing* no cliente.

4. **Nova secção `## Pedidos Efémeros (*Ephemeral Request Envelope*)`**, logo após o diagrama de consulta, com:
   - ciclo de vida em 6 passos (geração → assinatura → transporte → validação → resposta → consumo);
   - garantias: *forward secrecy*, raio de explosão mínimo, anti-replay, anti-correlação, coerência do backend zero-knowledge;
   - regras de implementação (nunca reutilizar, nunca escrever a privada em disco/log, um envelope = uma resposta, TTL *fail-closed*, servidor guarda só o *hash* do nonce);
   - uma subsecção **"O que os pedidos efémeros *não* resolvem"** (autenticação, anonimato, cliente comprometido, disponibilidade) — isto evita que o conceito seja lido como substituto do [[mTLS]]/[[WireGuard]];
   - um diagrama de sequência dedicado ao envelope.

5. **Cadeia de Auditoria Global** — novo evento `REQUEST_ENVELOPE` com `request_id`, *hash* do nonce, `case_id`, resultado da validação e autor, explicitando que a chave privada efémera nunca é registada.

6. **Depósito de prova (Fase 2)** — o transporte do payload passou a usar o mesmo tipo de envelope no sentido de escrita.

7. **Notas de Coerência** — novo ponto a fixar a distinção entre camada de autenticação (túnel) e camada E2EE (envelope), e a assinatura da chave efémera como defesa contra *key substitution*.

## Propagação para os documentos ligados

- **[[Requesitos]]** — criei **RNS13 — Envelope de Pedido Efémero com Forward Secrecy**, que formaliza o par X25519 por pedido, o nonce/TTL, a assinatura do *transcript*, a cifragem da resposta e o *zeroing*.
- **[[Ataques e Vetores]]** — enriqueci as linhas v1/v1a do Fluxo 2 e acrescentei **v1b** (replay de nonce/TTL), além de um parágrafo de enquadramento com link para a nova secção.

Se preferires que o RNS13 fique noutra numeração ou que os pedidos efémeros se apliquem também ao depósito (e não só à leitura), digo-me e ajusto.
[Timestamp: 2026/11/19 21:50:28]

**user**: explicacao do que é
[Timestamp: 2026/11/19 22:06:20]

**ai**: ## O que é um "pedido efémero"

É uma forma de fazer **um único pedido** ao sistema usando **material criptográfico que nasce e morre com esse pedido**.

A ideia central: em vez de o cliente ter *uma* chave de sessão que dura o dia todo e serve para todas as consultas, cada consulta traz um par de chaves novo, criado naquele momento, usado uma vez e apagado a seguir.

**Analogia:** é como enviar uma carta dentro de um envelope que só pode ser aberto por quem o recebe, e que é destruído depois de aberto — em vez de usares a mesma caixa de correio e a mesma chave durante semanas.

## Que problema resolve

Sem isto, a consulta típica funciona assim:

- o cliente e o servidor têm uma chave de sessão duradoura;
- todas as consultas dessa sessão usam a mesma chave;
- se alguém capturar tráfego e mais tarde obtiver essa chave (ou comprometer o servidor), **todas** as consultas antigas ficam legíveis.

Com pedidos efémeros:

- cada consulta tem a sua própria chave;
- capturar o tráfego de hoje não ajuda a abrir a consulta de amanhã;
- comprometer uma chave afeta **um** pedido, não a sessão inteira.

## Como funciona, em linguagem simples

1. **O cliente inventa uma chave nova** (par X25519) só para aquele pedido. A parte privada fica em RAM, nunca em disco.
2. **Junta os dados do pedido** — identificador, nonce (número de uso único), hora, validade curta, caso, chave pública — e **assina tudo** com a chave de identidade ([[Ed25519]]). A assinatura é o que impede alguém no meio (incluindo o backend) de trocar a chave pública por outra e ler a resposta.
3. **O servidor valida**: assinatura correta? certificado não revogado? pedido dentro do prazo? nonce já visto? Se algo falhar, rejeita.
4. **A resposta é cifrada para aquela chave pública** — e só essa chave privada, que existe apenas no cliente, a consegue abrir. O backend transporta bytes que não sabe ler.
5. **O cliente abre, usa e apaga** a chave privada efémera e o segredo partilhado.

## Porque é que a assinatura e o nonce são obrigatórios

- **Assinatura do envelope:** sem ela, o pedido efémero seria pior do que nada — o servidor (ou um intermediário) poderia substituir a tua chave pública pela dele e cifrar a resposta para si mesmo. A assinatura liga a chave efémera à tua identidade autenticada.
- **Nonce + TTL:** sem eles, um pedido capturado poderia ser reenviado e obter uma segunda resposta (*replay*). O nonce garante que cada pedido só é aceite uma vez; o TTL garante que um pedido antigo não serve mais tarde.

## O que isto **não** é

- **Não é autenticação.** Quem prova quem tu és é o certificado no túnel [[mTLS]]/[[WireGuard]]; o envelope efémero só diz *para onde cifrar a resposta*.
- **Não é anonimato.** O teu certificado continua a identificar-te.
- **Não protege um cliente já comprometido.** Se o telemóvel estiver infetado no momento da consulta, a chave está em RAM e pode ser lida — para isso existe o TPM/Secure Enclave.

## Em uma frase

> Um pedido efémero é um envelope de uso único: chave nova, assinada, com prazo e nonce, que garante que cada consulta é confidencial por si só e que o servidor nunca consegue ler a resposta que transporta.

Se quiseres, acrescento este texto como abertura da secção `## Pedidos Efémeros` no documento (antes do ciclo de vida), para quem lê pela primeira vez perceber o conceito antes dos detalhes técnicos.
[Timestamp: 2026/11/19 22:06:20]

**user**: X25519?
[Timestamp: 2026/11/19 22:12:20]

**ai**: ## O que é o X25519

**X25519 é uma função de troca de chaves Diffie-Hellman sobre a curva elíptica Curve25519** (definida no RFC 7748). Não cifra nada por si só e não assina nada: o que produz é um **segredo partilhado** entre duas partes, a partir das chaves públicas uma da outra.

- **Curve25519** é a curva, proposta por Daniel J. Bernstein (2005).
- **X25519** é a operação DH nessa curva, usando a forma de Montgomery (daí o "X", a coordenada x).
- **Chaves de 32 bytes**, segredo resultante também de 32 bytes.
- **Segurança ≈ 128 bits** (equivalente prático a RSA-3072, mas com chaves ~100× mais pequenas).
- Foi desenhada para ser **resistente a ataques de canal lateral**: implementação constante no tempo, sem ramificações dependentes de dados.

## Como se usa (e porque aparece no teu documento)

O X25519 sozinho não dá confidencialidade — dá um segredo. O padrão é este encadeamento:

```
par efémero X25519  →  ECDH  →  segredo partilhado
       ↓
   HKDF-SHA-256  →  chave simétrica
       ↓
 ChaCha20-Poly1305  →  dados cifrados
```

É exatamente isto que o documento descreve no envelope de resposta: o enclave gera um par efémero, faz ECDH com a chave pública efémera do cliente, deriva a chave com HKDF e cifra com ChaCha20-Poly1305.

**Efemeralidade** é a parte importante para o teu caso: se o par for gerado **por pedido** e destruído a seguir, obtém-se *forward secrecy* — comprometer a chave de identidade mais tarde não abre consultas antigas.

## X25519 vs. Ed25519 (a confusão habitual)

| | **X25519** | **Ed25519** |
| --- | --- | --- |
| Função | Troca de chaves (ECDH) | Assinatura digital |
| Curva | Curve25519 (Montgomery) | Edwards25519 (torcida) |
| Dá | Segredo partilhado | Assinatura de 64 bytes |
| Uso no sistema | Envelope dos pedidos | Assinar provas, notas, blocos, *transcript* |
| Tamanho da chave | 32 bytes | 32 bytes |

São "primos": a mesma família de curva, o mesmo nível de segurança (~128 bits), mas **funções diferentes**. E as chaves **não devem ser reutilizadas** entre as duas — um par Ed25519 não é um par X25519.

## Porque é a escolha certa aqui

- **Já está no stack:** o [[WireGuard]] usa Curve25519 nos seus handshakes (Noise IK). Usar X25519 no envelope mantém um só primitivo de curvas no sistema.
- **Leve para o terreno:** 32 bytes, rápido em dispositivos móveis e links satélite.
- **Sem parâmetros para negociar:** não há "qual curva?" — o que evita *downgrade*.
- **RSA não serve para isto:** transporte de chave com RSA é pesado, com chaves grandes, e não dá *forward secrecy* sem material efémero adicional.

## A limitação que obriga à assinatura

O ECDH é **não autenticado**. Sozinho, é vulnerável a *man-in-the-middle*: um atacante troca a chave pública e fica no meio.

É precisamente por isso que, no documento, o *transcript* do pedido (`request_id`, caso, nonce, timestamp, chave pública efémera) é **assinado com [[Ed25519]]**. O X25519 dá confidencialidade; a assinatura dá autoria. Um sem o outro não chega.

## Nota de longo prazo (relevante para provas forenses)

O X25519 **não é pós-quântico**. Para evidências que têm de permanecer secretas durante décadas, o risco é *harvest-now-decrypt-later*: tráfego capturado hoje pode ser decifrado quando existir um computador quântico. A mitigação padrão é um esquema **híbrido** — X25519 + ML-KEM (Kyber) — em que o segredo final é a combinação dos dois. Não é necessário para o trabalho agora, mas é o tipo de decisão que faz sentido referir como trabalho futuro.

Se quiseres, crio uma nota [[X25519]] no `batevidence/` (ao estilo de [[Ed25519]] e [[RSA]]) e passo as menções no documento de texto simples para link.
[Timestamp: 2026/11/19 22:12:20]