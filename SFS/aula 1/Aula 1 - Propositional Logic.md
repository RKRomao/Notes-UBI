# Aula 1 — Lógica Proposicional

📄 PDF: [[Aula 1 - Propositional Logic.pdf]]

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
- $\neg(p \land q) \equiv \neg p \lor \neg q$ (De Morgan)
- $\neg(p \lor q) \equiv \neg p \land \neg q$
- $p \leftrightarrow q \equiv (p \to q) \land (q \to p)$

## Formas normais
- **CNF**: conjunção de cláusulas (disjunções de literais) — usada por SAT.
- **DNF**: disjunção de conjunções.
- **Conversão para CNF**: eliminar $\to/\leftrightarrow$ → empurrar $\neg$ para dentro → distribuir $\lor$ sobre $\land$.

## Exercício
CNF de $p \to (q \land r)$:
$\neg p \lor (q \land r) \equiv (\neg p \lor q) \land (\neg p \lor r)$.

## Ver também
[[logica proposicional]] · [[logica condicional]] · [[SAT]]
