# Lógica Temporal

> Lógica para raciocinar sobre o **comportamento ao longo do tempo** de sistemas.
> Base das ferramentas de **model checking** em segurança e fiabilidade de software.

## 1. Porquê?
A lógica clássica diz se uma propriedade é verdadeira **agora**. Não consegue exprimir:
- "**nunca** acontece um deadlock";
- "**eventualmente** o pedido é respondido";
- "**sempre que** há um pedido, **mais tarde** há resposta".

A lógica temporal acrescenta **operadores temporais**.

## 2. Modelo de tempo
O sistema é descrito por uma **estrutura de Kripke**:
$$
\mathcal{K} = \langle S, S_0, R, L \rangle
$$
- $S$: estados;
- $S_0 \subseteq S$: estados iniciais;
- $R \subseteq S \times S$: transições;
- $L: S \to 2^{AP}$: propriedades atómicas verdadeiras em cada estado.

Um **caminho** (path) é uma sequência infinita de estados $\pi = s_0, s_1, s_2, \dots$ com $(s_i, s_{i+1}) \in R$.

## 3. LTL — Linear Temporal Logic
O tempo é **linear**: cada execução é uma sequência.

### Operadores
| Símbolo                        | Letra | Nome                | Significado                                    |
| ------------------------------ | ----- | ------------------- | ---------------------------------------------- |
| $\bigcirc \varphi$             | $X$   | next                | $\varphi$ é verdadeiro no **próximo** estado   |
| $\varphi\, \mathcal{U}\, \psi$ | $U$   | until               | $\varphi$ vale até $\psi$ se tornar verdadeiro |
| $\Diamond \varphi$             | $F$   | eventually / future | $\varphi$ é verdadeiro **algures** no futuro   |
| $\Box \varphi$                 | $G$   | always / globally   | $\varphi$ é verdadeiro **sempre**              |

Notação com letras (usada em texto e em ferramentas como NuSMV):

- $X\varphi$ — *neXt*: $\varphi$ no estado seguinte;
- $F\varphi$ — *Future* / eventually: $\varphi$ acaba por acontecer;
- $G\varphi$ — *Globally*: $\varphi$ vale sempre;
- $\varphi\,U\,\psi$ — *Until*: $\varphi$ mantém-se até $\psi$.

Variantes úteis:
- $W$ — *weak until*: $\varphi\,W\,\psi \equiv (\varphi\,U\,\psi) \lor G\varphi$ (não exige que $\psi$ aconteça).
- $R$ — *release*: $\psi\,R\,\varphi \equiv \neg(\neg\varphi\,U\,\neg\psi)$ (dual de $U$).

Derivações:
- $\Diamond \varphi \equiv \top\, \mathcal{U}\, \varphi$, ou seja $F\varphi \equiv \top\,U\,\varphi$
- $\Box \varphi \equiv \neg \Diamond \neg \varphi$, ou seja $G\varphi \equiv \neg F\neg\varphi$

### Exemplos
- **Safety**: nunca ocorre erro.
  $\Box \neg \text{Erro}$
- **Liveness**: eventualmente a mensagem é entregue.
  $\Diamond \text{Entregue}$
- **Response**: sempre que há pedido, eventualmente há resposta.
  $\Box(\text{Pedido} \to \Diamond \text{Resposta})$
- **Fairness**: se um processo pede, eventualmente entra na secção crítica.
  $\Box(\text{Pede}_1 \to \Diamond \text{CS}_1)$

Em notação de letras: $G\neg\text{Erro}$, $F\,\text{Entregue}$, $G(\text{Pedido} \to F\,\text{Resposta})$, $G(\text{Pede}_1 \to F\,\text{CS}_1)$.

### Semântica (esboço)
$\pi \models \Box \varphi$ se $\varphi$ vale em todos os estados de $\pi$.
$\pi \models \Diamond \varphi$ se $\varphi$ vale em algum estado de $\pi$.
$\pi \models \varphi\, \mathcal{U}\, \psi$ se existe $i$ com $\psi$ em $\pi_i$ e $\varphi$ em todos os $\pi_j$, $j<i$.

## 4. CTL — Computation Tree Logic
O tempo é **ramificado**: a partir de um estado há vários futuros possíveis.

### Quantificadores de caminho
- $A$: **em todos** os caminhos;
- $E$: **existe** um caminho.

### Operadores
- $AX\,\varphi$ — em todos os caminhos, $\varphi$ no próximo estado.
- $EX\,\varphi$ — existe caminho com $\varphi$ no próximo.
- $AF\,\varphi$ — em todos os caminhos, eventualmente $\varphi$.
- $EF\,\varphi$ — existe caminho onde eventualmente $\varphi$.
- $AG\,\varphi$ — em todos os caminhos, sempre $\varphi$.
- $EG\,\varphi$ — existe caminho onde sempre $\varphi$.
- $A[\varphi\,U\,\psi]$, $E[\varphi\,U\,\psi]$.

### Exemplos
- **Safety**: $AG\,\neg\text{Erro}$.
- **Possibilidade de recuperação**: $EF\,\text{Recuperado}$.
- **Inevitabilidade**: $AF\,\text{Terminado}$.

### LTL vs. CTL
- Nem todas as fórmulas LTL são exprimíveis em CTL e vice-versa.
- $\Box\Diamond p$ (LTL) não tem equivalente direto em CTL.
- $AG(EF\,p)$ (CTL) não é exprimível em LTL.
- **CTL*** une ambos.

## 5. Model Checking
Procedimento automático:
1. Modelar o sistema como estrutura de Kripke.
2. Especificar a propriedade em LTL/CTL.
3. Verificar algoritmicamente todos os estados.

### Algoritmos
- **CTL**: rotulagem de estados (fixed-point), tempo $O(|S| \cdot |\varphi|)$.
- **LTL**: tradução para autómato de Büchi + verificação de produto vazio.
- Ferramentas: **SPIN**, **NuSMV**, **PRISM**, **TLA+**.

### Explosão de estados
O número de estados cresce exponencialmente. Técnicas: redução de ordem parcial, abstração, BDDs, SAT-based model checking (BMC).

## 6. Propriedades importantes
| Tipo | Intuição | Exemplo |
|------|----------|---------|
| **Safety** | "nada mau acontece" | $\Box \neg \text{Deadlock}$ |
| **Liveness** | "algo bom eventualmente acontece" | $\Diamond \text{Resposta}$ |
| **Fairness** | condições justas de execução | $\Box\Diamond \text{Enabled} \to \Box\Diamond \text{Executed}$ |

## 7. Ligação a SFS
- Verificação de protocolos, concorrência, sistemas embebidos.
- Complementa [[SAT]]/[[SMT]]: estes verificam estados individuais; a lógica temporal verifica **sequências** de estados.
- **Bounded Model Checking** reduz a verificação temporal a SAT.

## 8. Exercícios
1. Exprime em LTL: "se o semáforo fica verde, eventualmente fica vermelho."
   $\Box(\text{Verde} \to \Diamond \text{Vermelho})$.
2. Exprime em CTL: "é possível alcançar um estado de sucesso."
   $EF\,\text{Sucesso}$.
3. Nega $\Box \Diamond p$ em LTL.
   $\Diamond \Box \neg p$.

## Ver também
[[logica proposicional]] · [[logica de primeiro grau]] · [[SAT]] · [[SMT]] · [[sistema de transicao]] · [[estrutura de kripke]] · [[algoritmo de model checking]]
