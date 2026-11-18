# Bit Blasting

## Ideia
Traduzir fórmulas sobre inteiros/bit-vetores para lógica proposicional (SAT), substituindo cada bit por uma variável booleana.

## Como funciona
1. Variável de $n$ bits → $n$ variáveis booleanas.
2. Operações aritméticas/lógicas → circuitos booleanos (somadores, comparadores).
3. Resultado em CNF para um solver SAT.

## Exemplo — soma de 1 bit
$s = a \oplus b$ (XOR), $c_{out} = a \land b$ (AND).
Em CNF:
- $s \leftrightarrow (a \lor b) \land \neg(a \land b)$
- $c_{out} \leftrightarrow a \land b$

## Comparações
$x < y$ em $n$ bits compara do bit mais significativo para o menos, com cláusulas de "menor até agora".

## Vantagens
- Reutiliza solvers SAT muito otimizados.
- Simples e correto.

## Desvantagens
- Explosão no número de variáveis (largura × operações).
- Perde estrutura aritmética (SMT com teoria BV pode ser mais eficiente).

## Uso
Hardware (BMC), SMT para bit-vectors, verificação de programas com inteiros de largura fixa.
