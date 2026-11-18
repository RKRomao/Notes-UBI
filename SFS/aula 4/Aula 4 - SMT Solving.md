# Aula 4 — SMT Solving

📄 PDF: [[Aula 4 - SMT Solving.pdf]] · Notebook: [[futoshiki-bitvector.ipynb]]

## Definição
**SMT** = SAT + teorias. Decide fórmulas de primeira ordem com aritmética, arrays, bit-vetores e funções não interpretadas.

## Arquitetura DPLL(T)
- **SAT solver** propõe atribuições booleanas.
- **T-solver** verifica consistência na teoria.
- Conflitos geram **lemmas** para o SAT.
- Combinação de teorias: **Nelson-Oppen**.

## Teorias comuns
| Teoria | Conteúdo |
|--------|----------|
| QF_UF | igualdade e funções não interpretadas |
| QF_LIA | aritmética linear inteira |
| QF_LRA | aritmética linear real |
| QF_BV | bit-vetores (por vezes via [[bit blasting]]) |
| QF_AX | arrays |

## Exemplo
$x > 0 \land y = x + 1 \land y < 3$ → $x=1, y=2$ (satisfazível).
$x > 0 \land x < 0$ → UNSAT.

## Ferramentas
**Z3** (Microsoft), CVC5, Yices.

## Aplicações
Verificação de programas, geração de testes, análise de segurança.

## Ver também
[[SMT]] · [[SAT]] · [[bit blasting]]
