## Conceitos-chave

- [[logica proposicional]] — sistema formal com proposições (V/F) e conectivos; base de SAT.
- [[logica condicional]] — regras se-então; modela dependências e invariantes.
- [[logica de primeiro grau]] — objetos, predicados, funções e quantificadores ($\forall, \exists$).
- [[logica temporal]] — tempo, estados e sequências: LTL, CTL e model checking.
- [[bit blasting]] — reduz fórmulas sobre bit-vetores/inteiros a SAT, bit a bit.
- [[SAT]] — decidir se uma fórmula booleana é satisfazível.
- [[SMT]] — SAT + teorias (aritmética, arrays, bit-vetores).
- [[sistema de transicao]] — estados, ações e relação de transição; modelo do comportamento do sistema.
- [[estrutura de kripke]] — grafo de estados rotulados; semântica de LTL/CTL.
- [[algoritmo de model checking]] — verificação exaustiva de CTL (rotulagem/pontos fixos) e LTL (autómatos de Büchi), BMC.

## Aulas

| Nº | Título | Apontamentos | PDF |
|----|--------|--------------|-----|
| 1 | Introdução da UC | [[Aula 1 - Introdução da UC]] | [[Aula 1 - Introdução da UC.pdf]] |
| 1 | Propositional Logic | [[Aula 1 - Propositional Logic]] | [[Aula 1 - Propositional Logic.pdf]] |
| 2 | SAT Solving | [[Aula 2 - SAT Solving]] | [[Aula 2 - SAT Solving.pdf]] |
| 3 | First-order Logic | [[Aula 3 - First-order Logic]] · [[logica de primeiro grau]] | [[Aula 3 - First-order Logic.pdf]] |
| 4 | SMT Solving | [[Aula 4 - SMT Solving]] | [[Aula 4 - SMT Solving.pdf]] |

### Diferença entre Security (Segurança), Safety (Segurança) e Reliability (Fiabilidade)

Apesar de em português "segurança" cobrir os dois primeiros, são propriedades distintas:

| Propriedade                       | Fonte da ameaça                    | Pergunta central                                       | Exemplo                                    |
| --------------------------------- | ---------------------------------- | ------------------------------------------------------ | ------------------------------------------ |
| **Safety** (segurança funcional)  | acidentes, falhas, ambiente físico | "nada de mau **acontece**?"                            | um reator nunca atinge temperatura crítica |
| **Security** (segurança/proteção) | **adversário intencional**         | "estamos protegidos de ataques?"                       | um atacante não lê nem altera mensagens    |
| **Reliability** (fiabilidade)     | falhas aleatórias de componentes   | "o serviço funciona **continuamente** e corretamente?" | o sistema fica disponível 99,9% do tempo   |

- **Safety** é uma propriedade de *ausência de estados perigosos* — tipicamente **invariantes** do tipo $\Box\neg\text{Perigo}$ em [[logica temporal]]; verifica-se com [[algoritmo de model checking]].
- **Security** assume um **modelo de ameaça** (confidencialidade, integridade, autenticidade, disponibilidade). Ferramentas: análise criptográfica, verificação de protocolos, *taint analysis*, *fuzzing*.
- **Reliability** é quantitativa: MTBF, taxa de falhas, disponibilidade. Modela-se com cadeias de Markov ou lógica temporal probabilística.

Relações:
- **Safety ≠ Security**: um sistema pode ser seguro contra acidentes e vulnerável a ataques (e vice-versa).
- **Security não implica Safety**: um ataque bem-sucedido pode causar uma violação de safety; daí o conceito de *security-informed safety*.
- **Safety não implica Security**: um sistema sem acidentes pode ser totalmente vulnerável a intrusões.
- **Reliability** é uma medida de **frequência**; **safety/security** são propriedades **binárias** (violadas ou não) no modelo.
- Em SFS, [[SAT]], [[SMT]] e [[logica temporal]] servem sobretudo para **safety e verificação funcional**; security usa ainda técnicas criptográficas e de análise de protocolos.

### Notebooks
- [[feature-model-z3.ipynb]]
- [[graph_colouring.ipynb]]
- [[futoshiki-bitvector.ipynb]]
