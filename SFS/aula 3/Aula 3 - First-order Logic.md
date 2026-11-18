# Aula 3 — Lógica de Primeira Ordem (FOL)

📄 PDF: [[Aula 3 - First-order Logic.pdf]]

## Porquê FOL?
A lógica proposicional não fala de objetos, relações nem quantificadores.

## Sintaxe
- **Termos**: variáveis, constantes, funções ($f(x)$).
- **Predicados**: $P(x)$, $R(x,y)$.
- **Quantificadores**: $\forall x\, P(x)$ (para todo), $\exists x\, P(x)$ (existe).
- **Conectivos**: os mesmos da proposicional.

## Semântica
Uma **interpretação** (domínio + atribuição de funções e predicados) dá significado.
- **Válida**: verdadeira em todas as interpretações.
- **Satisfazível**: verdadeira em pelo menos uma.

## Exemplo
$\forall x\, (Humano(x) \to Mortal(x))$, $Humano(Socrates)$ $\Rightarrow$ $Mortal(Socrates)$.

## Indecidibilidade
FOL é **indecidível** (Church-Turing). Fragmentos decidíveis são usados em [[SMT]].

## Ver também
[[logica de primeiro grau]] · [[logica proposicional]] · [[SMT]]
