> [!info] Definição
> Esta nota é um **índice** das explicações sobre tipos de ataque usadas no projeto BatEvidence. Cada tipo tem um ficheiro próprio, escrito para quem nunca ouviu falar do assunto: o que é, uma analogia, como funciona, um exemplo no sistema, o impacto e as mitigações.

## Índice

| Tipo de ataque | Em uma frase | Vetor(es) | Teste(s) |
| --- | --- | --- | --- |
| [[IDOR]] | Trocar o identificador de um objeto e aceder ao que não é seu. | v8, v9 | TC-RF11-02 |
| [[Escalada de Privilégios]] | Obter mais permissões do que as atribuídas. | v4, v8 | TC-RF11-03, TC-RNS19-01/02 |
| [[Ataque de Repetição (Replay)]] | Reenviar uma mensagem válida para repetir o efeito. | v1b | TC-RNS13-02, TC-Q-09 |
| [[Man-in-the-Middle (MitM)]] | Colocar-se entre duas partes e ler/alterar a conversa. | v2, v3 | TC-RNS01-01, TC-RNS14-01 |
| [[Ataque de Downgrade]] | Forçar o uso de uma versão ou algoritmo mais fraco. | v5, v7 | TC-RNS14-02 |
| [[Substituição de Chave (Key Substitution)]] | Trocar a chave pública da vítima pela do atacante. | v1b | TC-RNS13-01 |
| [[Falsificação de Assinatura]] | Fazer passar uma mensagem como assinada por outro. | v10 | TC-RNS08-01 |
| [[Adulteração de Registos (Tampering)]] | Alterar ou apagar dados/logs para esconder rastos. | v6, v6a | TC-RF12-01/02, TC-RNS20-01 |
| [[Injeção de Frações (Share Injection)]] | Submeter frações falsas na reconstrução Shamir. | v9 | TC-Q-06 |
| [[Exfiltração de Dados e Chaves]] | Copiar dados ou chaves para fora do sistema. | v1a, v11 | TC-RNS03-01, TC-RNS04-01 |
| [[Movimento Lateral]] | Passar de um sistema comprometido para outros. | v3, v4 | TC-RNS18-01 |
| [[Phishing e Spoofing de Interface]] | Enganar a pessoa com uma app ou ecrã falso. | v15, v16 | TC-RF20-01, TC-RNS10-01 |
| [[Negação de Serviço (DoS)]] | Tornar o serviço indisponível por esgotamento de recursos. | v2 | TC-RNS17-01, TC-RNS22-01 |

## Como ler cada nota

1. **Definição** — o que é, em uma ou duas frases.
2. **Em linguagem simples** — uma analogia do dia a dia.
3. **Como o ataque funciona** — os passos do atacante.
4. **Exemplo no BatEvidence** — onde o risco aparece no projeto.
5. **Porque é perigoso** — que garantia é afetada.
6. **Como o BatEvidence se protege** — requisitos e mecanismos.
7. **Como testar** — casos de [[Intregavel 4]] e vetores de [[Ataques e Vetores]].

_Referências Relacionadas:_ [[Requesitos]], [[Intregavel 4]], [[Ataques e Vetores]], [[Bat (couf couf) Evidence A Wayne]]
