# Lógica de Primeiro Grau (FOL)

> Também chamada **lógica de predicados** ou **lógica de primeira ordem**.
> Aula: [[Aula 3 - First-order Logic]] · PDF: [[Aula 3 - First-order Logic.pdf]]

## 1. Porquê?
A [[logica proposicional]] trata afirmações como blocos indivisíveis: "$p$ = está a chover". Não consegue exprimir:
- "**todos** os alunos passam se estudarem";
- "**existe** um número maior que todos";
- relações entre objetos: "$x < y$".

A FOL acrescenta **objetos**, **predicados**, **funções** e **quantificadores**.

## 2. Sintaxe

### Alfabeto
| Elemento        | Exemplo                                   |
| --------------- | ----------------------------------------- |
| Variáveis       | $x, y, z$                                 |
| Constantes      | $a, b, 0, \text{Socrates}$                |
| Funções         | $f(x)$, $+(x,y)$, $\text{pai}(x)$         |
| Predicados      | $P(x)$, $R(x,y)$, $x < y$                 |
| Conectivos      | $\neg, \land, \lor, \to, \leftrightarrow$ |
| Quantificadores | $\forall, \exists$                        |
| Parênteses      | $( )$                                     |

### Termos
Um **termo** representa um objeto:
- variável é termo;
- constante é termo;
- se $f$ é função de aridade $n$ e $t_1,\dots,t_n$ são termos, $f(t_1,\dots,t_n)$ é termo.

### Fórmulas (bem formadas)
1. Se $P$ é predicado de aridade $n$ e $t_1,\dots,t_n$ são termos, $P(t_1,\dots,t_n)$ é uma **fórmula atómica**.
2. Se $F$ e $G$ são fórmulas, então $\neg F$, $(F \land G)$, $(F \lor G)$, $(F \to G)$, $(F \leftrightarrow G)$ são fórmulas.
3. Se $F$ é fórmula e $x$ variável, então $\forall x\,F$ e $\exists x\,F$ são fórmulas.
4. Nada mais é fórmula.

### Variáveis livres e ligadas
- Em $\forall x\, P(x,y)$, $x$ está **ligada** (pelo quantificador) e $y$ está **livre**.
- Uma fórmula **fechada** (sentença) não tem variáveis livres.

## 3. Semântica

Uma **interpretação** (ou estrutura) $\mathcal{I} = \langle D, \cdot^{\mathcal{I}}\rangle$:
- $D$: domínio (conjunto de objetos);
- cada constante → elemento de $D$;
- cada função → função sobre $D$;
- cada predicado → relação sobre $D$.

Uma fórmula é:
- **válida** (tautologia) se verdadeira em **todas** as interpretações;
- **satisfazível** se verdadeira em **pelo menos uma**;
- **insatisfazível** se falsa em todas.

### Quantificadores
- $\forall x\, P(x)$: $P$ vale para **todos** os elementos de $D$.
- $\exists x\, P(x)$: existe **pelo menos um** elemento de $D$ com $P$.

### Negação de quantificadores
- $\neg \forall x\, P(x) \equiv \exists x\, \neg P(x)$
- $\neg \exists x\, P(x) \equiv \forall x\, \neg P(x)$

## 4. Exemplos

### Silogismo clássico
$$
\forall x\,(\text{Humano}(x) \to \text{Mortal}(x)), \quad \text{Humano(Socrates)}
\;\Rightarrow\; \text{Mortal(Socrates)}
$$

### "Todos os alunos gostam de lógica"
$$
\forall x\,(\text{Aluno}(x) \to \text{Gosta}(x,\text{Lógica}))
$$

### "Existe um aluno que gosta de lógica"
$$
\exists x\,(\text{Aluno}(x) \land \text{Gosta}(x,\text{Lógica}))
$$

> Atenção: usar $\land$ com $\exists$ e $\to$ com $\forall$.
> "Todos os alunos gostam" **não** é $\forall x(\text{Aluno}(x) \land \text{Gosta}(x))$ — isso diz que tudo é aluno.

## 5. Equivalências úteis
- $\forall x\,(P(x) \land Q(x)) \equiv \forall x\,P(x) \land \forall x\,Q(x)$
- $\exists x\,(P(x) \lor Q(x)) \equiv \exists x\,P(x) \lor \exists x\,Q(x)$
- $\forall x\,P(x) \to \exists x\,P(x)$ (se o domínio não for vazio)
- Distribuições que **não** valem:
  - $\forall x\,(P(x) \lor Q(x)) \not\equiv \forall x\,P(x) \lor \forall x\,Q(x)$
  - $\exists x\,(P(x) \land Q(x)) \not\equiv \exists x\,P(x) \land \exists x\,Q(x)$

## 6. Forma clausal e Skolemização
Para usar FOL num solver:
1. Eliminar $\to$ e $\leftrightarrow$.
2. Mover $\neg$ para dentro.
3. **Skolemizar**: substituir $\exists$ por constantes/funções de Skolem.
4. Colocar na **forma normal prenex**: quantificadores à frente.
5. Converter a matriz em **CNF**.

## 7. Decidibilidade
- A FOL é **semidecidível**: se a fórmula é válida, um procedimento encontra prova; se não é, pode não terminar.
- É **indecidível** (Church-Turing, 1936).
- Fragmentos decidíveis: lógica monádica, lógica de descrições, aritmética de Presburger.

## 8. Ligação a SMT
Os solvers [[SMT]] usam fragmentos decidíveis de FOL:
- **QF_UF** — funções não interpretadas + igualdade;
- **QF_LIA/LRA** — aritmética;
- **QF_BV** — bit-vetores;
- **QF_AX** — arrays.

## 9. Exercícios
1. Traduz: "Nenhum aluno é professor."
   $\forall x\,(\text{Aluno}(x) \to \neg\text{Professor}(x))$.
2. Traduz: "Há um número primo par."
   $\exists x\,(\text{Primo}(x) \land \text{Par}(x))$ — verdadeiro, pois $x=2$ satisfaz.
3. Nega: $\forall x \exists y\, R(x,y)$.
   $\exists x \forall y\, \neg R(x,y)$.

## Ver também
[[Aula 3 - First-order Logic]] · [[logica proposicional]] · [[SMT]]
