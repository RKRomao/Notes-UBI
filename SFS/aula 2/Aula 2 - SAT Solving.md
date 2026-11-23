# Aula 2 — SAT Solving

PDF: [[Aula 2 - SAT Solving.pdf]] 
Notebook: [[graph_colouring.ipynb]]
[[Aula 2 - SAT Solving]]
## Definição
**SAT**: dada uma fórmula proposicional em CNF, existe atribuição de variáveis que a torna verdadeira?

## Conceitos
- **Literal**: $x$ ou $\neg x$.
- **Cláusula**: disjunção de literais.
- **CNF**: conjunção de cláusulas.
- **Modelo**: atribuição que satisfaz a fórmula.
- **UNSAT**: nenhuma atribuição satisfaz.

## Complexidade
SAT foi o **primeiro problema NP-completo** (Cook-Levin). Ainda assim, solvers modernos resolvem instâncias enormes.

## DPLL
1. **Propagação unitária** — cláusula com um só literal livre → forçá-lo.
2. **Decisão** — escolher variável e valor.
3. **Backtracking** em conflito.
4. Repetir até modelo ou prova de UNSAT.

## CDCL (solvers modernos)
- Aprende **cláusulas de conflito**.
- **Backjumping** em vez de backtracking cronológico.
- Heurísticas **VSIDS** e reinícios.
- Exemplos: MiniSat, Glucose, CaDiCaL.

## Exemplo
$(x \lor y) \land (\neg x \lor z) \land (\neg y \lor z)$.
$x=F$ → $y=V$ → $z=V$. Modelo: $x=F, y=V, z=V$.

## Exercício
Mostra que $(x) \land (\neg x \lor y) \land (\neg y)$ é UNSAT.
$x=V$ → $y=V$ → conflito com $\neg y$.

## Ver também
[[SAT]] · [[bit blasting]] · [[CDCL]]
