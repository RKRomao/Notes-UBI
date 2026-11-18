# SMT (Satisfiability Modulo Theories)

## Definição
Generalização do SAT: decide satisfazibilidade de fórmulas de primeira ordem **com teorias** (aritmética, arrays, bit-vetores, funções não interpretadas).

## Arquitetura DPLL(T)
- **SAT solver** propõe atribuições booleanas.
- **T-solver** verifica consistência na teoria.
- Conflitos geram **lemmas** (cláusulas) para o SAT.
- Combinação de teorias: **Nelson-Oppen** (com partilha de igualdades).

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
$x > 0 \land x < 0$ → UNSAT por inconsistência aritmética.

## Ferramentas
- **Z3** (Microsoft) — usado no notebook [[aula 4/futoshiki-bitvector.ipynb]].
- CVC5, Yices, Boogie.

## Aplicações
Verificação de programas, geração de testes, análise de segurança, síntese.

## Exercício
Codifica "todos os números de 1 a 9 são distintos" em SMT-LIB usando `distinct`.
