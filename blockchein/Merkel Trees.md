Uma **Árvore de Merkle** (_Merkle Tree_ / Árvore de Hash) é uma estrutura de dados fundamental na ciência da computação e na criptografia, utilizada para verificar a integridade e consistência de grandes conjuntos de dados de forma extremamente rápida e segura.

O nome vem do seu criador, Ralph Merkle, que patenteou o conceito em 1979.

## Como funciona a estrutura?

Uma Árvore de Merkle tem o formato de uma árvore invertida, construída de baixo para cima através de funções de hash (algoritmos que transformam qualquer dado num código alfanumérico de tamanho fixo):

1. **Blocos de Dados:** Os dados originais são divididos em pequenos blocos na base da árvore.
2. **Nós Folha (Leaf Nodes):** Cada bloco de dados passa por uma função de hash (como o SHA-256), gerando um hash único.
3. **Nós Intermediários (Non-Leaf Nodes):** Os hashes resultantes são agrupados em pares. O sistema concatena (junta) os dois hashes e aplica a função criptográfica novamente para criar um novo hash "pai".
4. **Raiz de Merkle (Merkle Root):** O processo de emparelhamento e geração de hashes repete-se subindo na árvore até sobrar apenas um único hash no topo. Este hash final representa criptograficamente **todos** os dados da árvore subjacente.

As Árvores de Merkle dividem-se em duas abordagens fundamentais: as [Clássicas (Dense)](Árvore%20de%20Merkle%20Clássica%20(Dense).md) e as **Esparsas (Sparse)**


Como posso verificar se existe ou nao na arvore 