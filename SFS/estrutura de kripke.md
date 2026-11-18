# Estrutura de Kripke

> Grafo de estados etiquetados com proposições atómicas — a semântica standard de [[logica temporal]] (LTL/CTL).
> É o modelo sobre o qual corre o [[algoritmo de model checking]].

## 1. Definição

$$
\mathcal{K} = \langle S,\ S_0,\ R,\ L \rangle
$$

| Componente | Significado |
|------------|-------------|
| $S$ | conjunto finito de **estados** |
| $S_0 \subseteq S$ | **estados iniciais** |
| $R \subseteq S \times S$ | **relação de transição**, normalmente **total** |
| $L : S \to 2^{AP}$ | **rotulagem**: proposições atómicas verdadeiras em cada estado |

**Totalidade**: para todo $s \in S$ existe $s'$ com $(s, s') \in R$. Deadlocks resolvem-se com auto-laços.

## 2. Caminhos

Um **caminho** a partir de $s_0$ é uma sequência infinita

$$
\pi = s_0, s_1, s_2, \dots \quad \text{com } (s_i, s_{i+1}) \in R
$$

O **traço** de $\pi$ é $L(s_0), L(s_1), \dots$ — as observações que o sistema produz.

O conjunto de todos os caminhos forma a **árvore de computação**: a raiz são os estados iniciais e cada nó tem como filhos os sucessores. É esta árvore que CTL quantifica ($A$ = todos os caminhos, $E$ = algum caminho).

## 3. Exemplo

Sistema com uma lâmpada e um interruptor: $S=\{s_0,s_1\}$, $S_0=\{s_0\}$,
$R=\{(s_0,s_1),(s_1,s_0)\}$, $AP=\{\text{on}\}$, $L(s_1)=\{\text{on}\}$, $L(s_0)=\varnothing$.

- $\mathcal{K}, s_0 \models AG(\text{on} \to AF\,\neg\text{on})$? Sim (alterna sempre).
- $\mathcal{K}, s_0 \models AF\,\text{on}$? Sim.
- $\mathcal{K}, s_0 \models AG\,\text{on}$? Não.

## 4. Semântica de LTL e CTL (esboço)

**LTL** — avalia-se por caminho:
- $\pi \models \Box\varphi$ se $\varphi$ vale em todos os estados de $\pi$;
- $\pi \models \Diamond\varphi$ se vale em algum;
- $\pi \models \bigcirc\varphi$ se vale no sucessor imediato;
- $\pi \models \varphi\,\mathcal{U}\,\psi$ se $\psi$ vale em $i$ e $\varphi$ em todos os $j<i$.

**CTL** — avalia-se por estado, com quantificadores de caminho:
- $s \models AX\,\varphi$ se todos os sucessores satisfazem $\varphi$;
- $s \models EX\,\varphi$ se algum sucessor satisfaz $\varphi$;
- $s \models AG\,\varphi$ / $AF\,\varphi$ / $EG\,\varphi$ / $EF\,\varphi$ conforme "sempre"/"eventualmente" em todos/alguns caminhos.

## 5. Relação com sistemas de transição

Um [[sistema de transicao]] $\langle S, Act, \to, I, AP, L\rangle$ converte-se numa estrutura de Kripke:

1. $S_0 := I$;
2. $R := \{(s,s') \mid \exists \alpha.\ s \xrightarrow{\alpha} s'\}$ — as **ações são esquecidas**;
3. totalizar $R$ acrescentando auto-laços nos estados terminais;
4. manter $L$.

As ações perdem-se, mas continuam a poder modelar-se com proposições atómicas (ex.: `a_req`, `b_cs`). Para raciocinar explicitamente sobre ações usam-se **sistemas de transição etiquetados** ou **autómatos de Büchi**.

## 6. Produto e verificação

Para LTL constrói-se o **produto** $\mathcal{K} \times \mathcal{A}_{\neg\varphi}$ com o autómato de Büchi da negação da propriedade:

- se o produto tiver um caminho de aceitação → **contraexemplo**;
- se não tiver → a propriedade vale em todos os caminhos.

Para CTL não é preciso produto: o [[algoritmo de model checking]] rotula os estados de $\mathcal{K}$ diretamente.

## 7. Explosão de estados

$|S|$ cresce exponencialmente com o número de componentes concorrentes (produto de estados locais) e com variáveis inteiras. Mitigações:

- **redução de ordem parcial** (LTL);
- **abstração** e *predicate abstraction*;
- **BDDs** e *symbolic model checking*;
- **SAT-based BMC** (ver [[SAT]]);
- **simetria** e *slicing*.

## 8. Exercícios

1. Constrói a estrutura de Kripke do algoritmo de Peterson para 2 processos e identifica os estados de exclusão mútua.
2. Mostra que $AG(p \to AF\,q)$ não é o mesmo que $AG(p \to q)$.
3. Porque é que a totalidade de $R$ simplifica a semântica de $\bigcirc$ e de $AX$?

## Ver também

[[logica temporal]] · [[sistema de transicao]] · [[algoritmo de model checking]] · [[SAT]] · [[SMT]]
