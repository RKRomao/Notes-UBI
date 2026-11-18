# Sistema de Transição

> Modelo abstrato do **comportamento** de um sistema: em cada estado, que ações pode executar e para onde vai.
> É o ponto de partida para construir uma [[estrutura de kripke]] e aplicar [[algoritmo de model checking]].

## 1. Definição

Um sistema de transição é um tuplo:

$$
\mathcal{T} = \langle S,\ Act,\ \to,\ I,\ AP,\ L \rangle
$$

| Componente | Significado |
|------------|-------------|
| $S$ | conjunto (finito) de **estados** |
| $Act$ | conjunto de **ações** |
| $\to\ \subseteq S \times Act \times S$ | **relação de transição**: $s \xrightarrow{\alpha} s'$ |
| $I \subseteq S$ | **estados iniciais** |
| $AP$ | conjunto de **proposições atómicas** |
| $L : S \to 2^{AP}$ | **função de rotulagem**: factos verdadeiros em cada estado |

Escreve-se $s \xrightarrow{\alpha} s'$ quando o sistema, no estado $s$, executa a ação $\alpha$ e passa a $s'$.

## 2. Execuções e caminhos

- **Caminho (path)**: sequência $s_0 \to s_1 \to s_2 \to \dots$ com $(s_i, s_{i+1}) \in \to$ (ignorando ações).
- **Execução (execution)**: caminho etiquetado com as ações, $s_0 \xrightarrow{\alpha_0} s_1 \xrightarrow{\alpha_1} \dots$.
- **Traço (trace)**: sequência de conjuntos de proposições atómicas $L(s_0), L(s_1), \dots$

Um caminho diz-se **maximal** se for infinito ou terminar num estado sem transições (deadlock).

## 3. Determinismo e não-determinismo

- **Determinístico**: para cada par $(s, \alpha)$ existe no máximo um $s'$.
- **Não-determinístico**: pode haver vários $s'$ para a mesma ação.
- **Reativo**: nunca termina — o comportamento relevante é o conjunto de caminhos infinitos.

O não-determinismo modela escolhas do ambiente, concorrência e valores de entrada desconhecidos.

## 4. Exemplo — semáforo

Estados $S = \{V, A, E\}$ (verde, amarelo, encarnado), $Act = \{\text{mudar}\}$:

$$
V \xrightarrow{\text{mudar}} A \xrightarrow{\text{mudar}} E \xrightarrow{\text{mudar}} V
$$

$I = \{V\}$, $AP = \{\text{verde}, \text{amarelo}, \text{encarnado}\}$, com $L(V) = \{\text{verde}\}$, etc.
Propriedade desejada: "nunca verde e encarnado ao mesmo tempo" — trivial aqui, mas o modelo cresce rapidamente com cruzamentos.

## 5. Concorrência por interleaving

Dois processos $P_1$ e $P_2$ combinam-se por **entrelaçamento**:

- transições de $P_1$ mantêm o estado de $P_2$ fixo, e vice-versa;
- o estado global é o par $(s_1, s_2)$;
- o número de estados é o **produto** dos estados locais — origem da **explosão de estados**.

Também se podem modelar **variáveis partilhadas** (ex.: `turn`, `flag[1]`, `flag[2]` no algoritmo de Peterson).

## 6. Sistemas de transição vs. estruturas de Kripke

| Sistema de transição | [[estrutura de kripke]] |
|----------------------|--------------------------|
| Tem **ações**: $s \xrightarrow{\alpha} s'$ | Só tem transições: $(s, s') \in R$ |
| Pode ter estados terminais | $R$ costuma ser **total** |
| Foco: ações, composição, protocolos | Foco: semântica de LTL/CTL |

Para obter uma estrutura de Kripke: apagar as ações, acrescentar estados iniciais e **totalizar** $R$ (auto-laços nos deadlocks).

## 7. Ligação a SFS

- Modela protocolos, algoritmos concorrentes, controladores e sistemas embebidos.
- Sobre ele se escrevem propriedades de **safety** ("nada mau acontece") e **liveness** ("algo bom acaba por acontecer") em [[logica temporal]].
- O [[algoritmo de model checking]] verifica essas propriedades exaustivamente sobre todos os caminhos.
- Técnicas de redução: **redução de ordem parcial**, **abstração**, **simetria**, **slicing**.

## 8. Exercícios

1. Modela um contador de 0 a 2 (módulo 3) como sistema de transição.
   $S=\{0,1,2\}$, $Act=\{\text{inc}\}$, $0 \xrightarrow{\text{inc}} 1 \xrightarrow{\text{inc}} 2 \xrightarrow{\text{inc}} 0$.
2. Modela duas máquinas de café que partilham um único depósito.
3. Porque é que a relação de transição de uma estrutura de Kripke deve ser total?

## Ver também

[[logica temporal]] · [[estrutura de kripke]] · [[algoritmo de model checking]] · [[SAT]] · [[SMT]]
