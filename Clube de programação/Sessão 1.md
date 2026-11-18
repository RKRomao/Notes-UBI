Pretende se aprender sobre Teoria de Grafos

A **teoria dos grafos** é um ramo da matemática e da ciência da computação que estuda estruturas chamadas **grafos**, formadas por conjuntos de **vértices** (nós) ligados por **arestas** (linhas), ideais para modelar redes de relacionamento, fluxos e caminhos.

## Conceitos Fundamentais

 **Vértice (ou Nó):**
Representa o objeto, ponto ou entidade da rede (como uma cidade ou uma pessoa).

**Aresta (ou Arco):** 
Representa a relação ou ligação entre dois vértices (como uma estrada ou uma amizade).

**Grafo Orientado:**
As arestas possuem uma direção definida (fluxo de um ponto para outro).

**Grafo Não Orientado:** 
As ligações funcionam em ambos os sentidos com igual valor.

**Grafo Ponderado:** 
As arestas possuem um "peso" ou custo associado (distância, tempo ou preço).

Programatico:
a representacao em programacao é feita atraves de matrizes.
cada ponto na matriz representa as suas ligacoes 
ex.

## Busca Binaria

Por cada busca, desconsidera metade dos dados. 


exercicio:
Given an integer array `nums`, return _the length of the longest **strictly increasing**_ _**subsequence**_.

**Example 1:**

**Input:** nums = [10,9,2,5,3,7,101,18]
**Output:** 4
**Explanation:** The longest increasing subsequence is [2,3,7,101], therefore the length is 4.

**Example 2:**

**Input:** nums = [0,1,0,3,2,3]
**Output:** 4

**Example 3:**

**Input:** nums = [7,7,7,7,7,7,7]
**Output:** 1

**Constraints:**

- `1 <= nums.length <= 2500`
- `-104 <= nums[i] <= 104`

[x1,x2,x3,x4,x5,x6,...,xn]

def	comprimento_crescente
	wile indice range (len(list))
		valor_inicial = list[indice]
		se o valor seguinte for maior
			valor_inicial = list[i]
			+1 a contagem
		se nao:
			passa a frente
	return contagem

indice = 0
list[] ->input
wile indice range (len(list))
	contagem comprimento_crescente(indice,list[indice])
	indice += contagem



def bla
	ver alista
		ver a lisata para cada
		final		
	max 
	if max < legthRec(list[])


Recursividade e DFS