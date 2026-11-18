# SAT (Boolean Satisfiability)

## Definição
Dada uma fórmula proposicional em CNF, existe atribuição de variáveis que a satisfaz?

## Conceitos
- **Literal**: $x$ ou $\neg x$.
- **Cláusula**: disjunção de literais.
- **CNF**: conjunção de cláusulas.
- **Modelo**: atribuição que satisfaz a fórmula.
- **UNSAT**: nenhuma atribuição satisfaz.

## Complexidade
SAT foi o **primeiro problema NP-completo** (Cook-Levin). Ainda assim, solvers resolvem instâncias reais com milhões de variáveis.

## DPLL
1. **Propagação unitária**: cláusula com um literal só → forçá-lo.
2. **Decisão**: escolher variável e valor.
3. **Backtracking** se conflito.
4. Repetir até modelo ou prova de UNSAT.

## CDCL (solver moderno)
- Aprende **cláusulas de conflito** (evita repetir erros).
- **Backjumping** em vez de backtracking cronológico.
- **Heurísticas** (VSIDS) e **reinícios**.
- Exemplos: MiniSat, Glucose, CaDiCaL.

## Exemplo
$(x \lor y) \land (\neg x \lor z) \land (\neg y \lor z)$.
Decisão $x=F$ → cláusula 1 força $y=V$ → cláusula 3 com $\neg y$ força $z=V$. Modelo: $x=F, y=V, z=V$.

## Aplicações
- Verificação de hardware/software.
- Planeamento e scheduling.
- Configuração de produtos.
- Coloração de grafos ([[aula 2/graph_colouring.ipynb]]).

## Exercício
Mostra que $(x) \land (\neg x \lor y) \land (\neg y)$ é UNSAT.
$x=V$ → $\neg x \lor y$ força $y=V$ → $\neg y$ conflito. Logo UNSAT.
