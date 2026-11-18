Árvore construída **apenas com os dados existentes**, de forma dinâmica. Se houver 8 transações ou blocos de ficheiros, a árvore terá exatamente 8 nós na base (folhas).

## Características
Os dados são adicionados **sequencialmente**, da esquerda para a direita.
A ordem de inserção pode ser arbitrária (aleatória) — o que importa é a sequência em que os blocos são colocados.
Cada par de folhas adjacentes é combinado num nó pai, repetindo o processo até restar um único hash no topo: a **Merkle Root**.
## Pontos fortes
**Prova de Inclusão eficiente**: para provar que um dado existe na árvore, basta fornecer o caminho da folha até à raiz (os hashes dos irmãos de cada nível). O verificador recalcula a raiz e compara com a conhecida.
## Limitações
**Prova de Não-Inclusão é cara**: para demonstrar que um dado **não** está na árvore, é necessário revelar ou verificar praticamente todas as folhas, mostrando que nenhuma corresponde ao que se procura.
Não há posições fixas nem "slots" vazios, a árvore cresce e encolhe com os dados.
## Quando usa

- Blocos de transações em blockchains (ex.: Bitcoin), onde apenas se prova inclusão.
- Cenários em que o conjunto de dados é estável e a não-inclusão não é necessária.

> Ver também: [[Busca_em_Merkel]] para os algoritmos de busca e provas.