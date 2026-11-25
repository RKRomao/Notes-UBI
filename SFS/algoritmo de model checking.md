> Verificação **exaustiva e automática** de que um modelo finito satisfaz uma propriedade temporal.
> Entrada: [[estrutura de kripke]] + fórmula de [[logica temporal]]. Saída: *true* ou **contraexemplo**.

## 1. Fluxo geral

1. **Modelar** o sistema como [[sistema de transicao]] e depois como [[estrutura de kripke]].
2. **Especificar** a propriedade em LTL ou CTL (safety, liveness, fairness).
3. **Verificar** algoritmicamente todos os estados/caminhos.
4. Se falhar, **analisar o contraexemplo** (traço que viola a propriedade).

## 2. Model checking de CTL — rotulagem de estados

Ideia: calcular, para cada subfórmula $\psi$, o conjunto $\text{Sat}(\psi) \subseteq S$ dos estados que a satisfazem.

```
SAT(φ):
  para cada subfórmula ψ de φ, por ordem crescente de complexidade:
     calcula Sat(ψ)
  devolve S0 ⊆ Sat(φ)
```

Casos:

| Fórmula | Cálculo |
|---------|---------|
| $p$ | $\{s \mid p \in L(s)\}$ |
| $\neg\psi$ | $S \setminus \text{Sat}(\psi)$ |
| $\psi_1 \land \psi_2$ | $\text{Sat}(\psi_1) \cap \text{Sat}(\psi_2)$ |
| $EX\,\psi$ | $\{s \mid \exists s'.\ (s,s') \in R \land s' \in \text{Sat}(\psi)\}$ |
| $EG\,\psi$ | maior subgrafo com todos os estados em $\text{Sat}(\psi)$ (ponto fixo) |
| $EU(\psi_1,\psi_2)$ | menor ponto fixo: $\psi_2 \cup (\psi_1 \cap EX(\cdot))$ |
| $AF\,\psi$ | $AX$ iterado até ponto fixo (menor) |
| $EF\,\psi$ | $EX$ iterado até ponto fixo (menor) |
| $AG\,\psi$ | $\neg EF\,\neg\psi$ |
| $AR$, $AU$ | duals de $EU$, $ER$ |

### Pontos fixos

- **Menor ponto fixo** ($\mu$): começar em $\varnothing$ e iterar até estabilizar — usado por $EF$, $AF$, $EU$.
- **Maior ponto fixo** ($\nu$): começar em $S$ e iterar até estabilizar — usado por $EG$, $AG$.

**Complexidade**: $O(|S| \cdot |\varphi|)$ — linear no tamanho do modelo e da fórmula.

## 3. Model checking de LTL — autómatos de Büchi

1. Traduzir $\neg\varphi$ num **autómato de Büchi** $\mathcal{A}_{\neg\varphi}$ (aceita exatamente os traços que violam $\varphi$).
2. Construir o **produto** $\mathcal{K} \times \mathcal{A}_{\neg\varphi}$.
3. Procurar um **caminho de aceitação** (que visita estados de aceitação infinitas vezes).

- Produto vazio → propriedade verificada.
- Caminho encontrado → contraexemplo (lasso: prefixo + ciclo).

**Complexidade**: $O(|S| \cdot 2^{|\varphi|})$ — exponencial na fórmula (tradução LTL→Büchi).

### Algoritmo de verificação de vazio

- **Nested DFS** (usado pelo SPIN): procura um ciclo alcançável que contenha um estado de aceitação.
- Alternativa: SCCs (componentes fortemente conexas) e verificar se alguma contém aceitação.

## 4. Bounded Model Checking (BMC)

Reduz a procura de contraexemplos de comprimento $\le k$ a [[SAT]]:

1. Desenrolar a estrutura de Kripke $k$ passos: variáveis $s_0, \dots, s_k$.
2. Codificar $R(s_i, s_{i+1})$ e $L(s_i)$ em CNF.
3. Codificar a negação da propriedade nos $k$ passos.
4. Se SAT → contraexemplo; se UNSAT → nada se conclui para além de $k$ (aumentar $k$ ou usar indução).

Vantagens: usa solvers maduros (ver [[SAT]], [[SMT]]); desvantagem: não é uma prova completa sem *k-induction* ou *interpolation*.

## 5. Comparação

| Aspeto         | CTL              | LTL                           |
| -------------- | ---------------- | ----------------------------- |
| Estrutura      | rotulagem direta | produto com autómato de Büchi |
| Complexidade   | $O(              | S                             |
| Contraexemplo  | caminho          | lasso (prefixo + ciclo)       |
| Expressividade | ramificada       | linear (não comparável)       |

## 6. Ferramentas

- **SPIN** — LTL, Promela, nested DFS.
- **NuSMV / NuXmv** — CTL e LTL, BDDs e SAT.
- **PRISM** — sistemas probabilísticos (DTMC/MDP).
- **TLA+ / TLC** — especificação de sistemas distribuídos.
- **CBMC / ESBMC** — BMC para código C.

## 7. Limitações

- **Explosão de estados**: $|S|$ cresce exponencialmente.
- Necessidade de um **modelo finito e correto** (a verificação vale para o modelo, não para o programa real).
- Propriedades de **liveness** exigem justiça (*fairness*) para evitar contraexemplos espúrios.

## 8. Exercícios

1. Aplica a rotulagem CTL a $AF\,p$ num modelo de 3 estados e indica o ponto fixo.
2. Porque é que $EG\,\varphi$ é um **maior** ponto fixo e $EF\,\varphi$ um **menor**?
3. Codifica em SAT o contraexemplo de $\Box\neg\text{erro}$ para $k=2$.
4. Constrói o autómato de Büchi para $\Diamond p$ e nega-o.

## Ver também

[[logica temporal]] · [[estrutura de kripke]] · [[sistema de transicao]] · [[SAT]] · [[SMT]]
