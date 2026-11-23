## Objetivo
Representar e raciocinar sobre afirmações V/F e verificar consistência.

## Sintaxe
- **Átomo**: $p, q, r, \dots$
- **Fórmula**: $\neg F$, $(F \land G)$, $(F \lor G)$, $(F \to G)$, $(F \leftrightarrow G)$.

## Precedência
$\neg$ > $\land$ > $\lor$ > $\to$ > $\leftrightarrow$.

## Semântica (tabelas de verdade)
| $p$ | $q$ | $\neg p$ | $p \land q$ | $p \lor q$ | $p \to q$ | $p \leftrightarrow q$ |
|-----|-----|----------|-------------|------------|-----------|------------------------|
| V | V | F | V | V | V | V |
| V | F | F | F | V | F | F |
| F | V | V | F | V | V | F |
| F | F | V | F | F | V | V |

$p \to q$ é falso **só** quando $p=V, q=F$.

## Equivalências
- $p \to q \equiv \neg p \lor q$
- $\neg(p \land q) \equiv \neg p \lor \neg q$
- $\neg(p \lor q) \equiv \neg p \land \neg q$
- $p \leftrightarrow q \equiv (p \to q) \land (q \to p)$

## Formas normais
- **CNF**: conjunção de cláusulas (cada cláusula é disjunção de literais).
- **DNF**: disjunção de conjunções.
- **Conversão para CNF**: eliminar $\to/\leftrightarrow$ → empurrar $\neg$ → distribuir $\lor$ sobre $\land$.

## Satisfazibilidade
Fórmula **satisfazível** se alguma atribuição a torna V;
**tautologia** se todas; **contraditória** se nenhuma.

## Exercícios
1. CNF de $p \to (q \land r)$: $(\neg p \lor q) \land (\neg p \lor r)$.
2. $(p \lor q) \land (\neg p \lor q) \land (p \lor \neg q)$ — satisfazível? Sim: $p=V, q=V$? Verifica: $p \lor q=V$; $\neg p \lor q=V$; $p \lor \neg q=V$. Modelo válido.
