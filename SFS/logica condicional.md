# Lógica Condicional

## O que é
Regras do tipo "se … então …", usadas para modelar dependências, pré-condições e invariantes.

## Implicação $p \to q$
- $p$: antecedente; $q$: consequente.
- Falsa apenas com $p=V, q=F$.
- Equivalente a $\neg p \lor q$.
- **Contrapositiva**: $p \to q \equiv \neg q \to \neg p$.
- **Recíproca** $q \to p$ **não** é equivalente!

## Bicondicional $p \leftrightarrow q$
Verdadeiro quando ambos têm o mesmo valor. Equivale a $(p \to q) \land (q \to p)$.

## Condições
- **Necessária**: $q$ é necessário para $p$: $p \to q$.
- **Suficiente**: $p$ é suficiente para $q$: $p \to q$.
- **Necessária e suficiente**: $p \leftrightarrow q$.

## Aplicação em software
- Pré-condição → pós-condição.
- Invariantes de ciclo: $I \land B \to I'$ (preservação).
- Contratos: `requires` / `ensures`.

## Exemplo
"Se saldo ≥ valor então autorizar": $Saldo \ge Valor \to Autorizar$.
Violação: saldo suficiente **e** não autorizar → $V \to F = F$.

## Exercício
Simplifica $(p \to q) \land (\neg q)$.
$\equiv (\neg p \lor q) \land \neg q \equiv \neg p$ (modus tollens).
