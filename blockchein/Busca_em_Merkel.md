# Busca em Árvore de Merkle (Sparse e Dense)

## Dense (Clássica)

Na árvore **dense**, as folhas são preenchidas sequencialmente com os dados existentes (sem posições vazias). A busca (prova de inclusão) segue o caminho da folha até à raiz, recolhendo os irmãos (*siblings*) de cada nível.

### Algoritmo — Prova de Inclusão

1. Localizar a folha correspondente ao dado (por índice ou hash).
2. Para cada nível, obter o hash do **irmão** da posição atual.
3. Calcular `hash(atual + irmão)` (ou `hash(irmão + atual)` conforme a ordem).
4. Repetir até chegar à raiz.
5. Comparar o hash calculado com a **Merkle Root** conhecida.

### Exemplo (Python)
```python
import hashlib

def H(x):
    return hashlib.sha256(x.encode()).hexdigest()

def build_dense(leaves):
    # leaves: lista de strings
    level = [H(l) for l in leaves]
    tree = [level]
    while len(level) > 1:
        next_level = []
        for i in range(0, len(level), 2):
            a = level[i]
            b = level[i+1] if i+1 < len(level) else a  # duplica se ímpar
            next_level.append(H(a + b))
        level = next_level
        tree.append(level)
    return tree

def proof_inclusion(tree, index):
    proof = []
    pos = index
    for level in tree[:-1]:
        sibling = pos ^ 1
        if sibling < len(level):
            proof.append((level[sibling], pos % 2 == 0))  # (hash, is_left)
        pos //= 2
    return proof

def verify_inclusion(leaf, proof, root):
    cur = H(leaf)
    for sib, is_left in proof:
        if is_left:
            cur = H(cur + sib)
        else:
            cur = H(sib + cur)
    return cur == root

# Uso
leaves = ["tx1", "tx2", "tx3", "tx4"]
tree = build_dense(leaves)
root = tree[-1][0]
proof = proof_inclusion(tree, 1)  # provar tx2
print("root:", root)
print("verificado:", verify_inclusion("tx2", proof, root))
```

### Limitação da Dense

- **Prova de não-inclusão é cara**: é necessário revelar/verificar praticamente todas as folhas para mostrar que um dado não está presente.

---

## Sparse (Esparsa)

Na árvore **sparse**, existe uma posição fixa para **cada** possível chave (ex.: espaço de endereços de 256 bits). Folhas vazias têm um hash padrão (ex.: `H("")` ou zero). Isto permite **provas de não-inclusão** baratas: basta mostrar que a posição da chave contém o hash vazio (ou um valor diferente).

### Algoritmo — Prova de Inclusão / Não-Inclusão

1. Calcular a posição (índice) da chave no espaço de endereços.
2. Percorrer do nível da folha até à raiz, recolhendo irmãos.
3. Na folha, se o valor for o hash vazio → **não está presente**; caso contrário, comparar com `H(dado)`.
4. Recalcular a raiz e comparar com a Merkle Root.

### Exemplo (Python, simplificado)

```python
import hashlib

EMPTY = hashlib.sha256(b"").hexdigest()

def H(x):
    return hashlib.sha256(x.encode()).hexdigest()

def sparse_root(leaves, depth=4):
    # leaves: dict {index: value_hash}
    # preenche posições em falta com EMPTY
    n = 1 << depth
    level = [leaves.get(i, EMPTY) for i in range(n)]
    while len(level) > 1:
        level = [H(level[i] + level[i+1]) for i in range(0, len(level), 2)]
    return level[0]

def sparse_proof(leaves, index, depth=4):
    n = 1 << depth
    level = [leaves.get(i, EMPTY) for i in range(n)]
    proof = []
    pos = index
    cur_level = level
    for _ in range(depth):
        sib = pos ^ 1
        proof.append(cur_level[sib])
        # subir
        next_level = [H(cur_level[i] + cur_level[i+1]) for i in range(0, len(cur_level), 2)]
        pos //= 2
        cur_level = next_level
    return proof

def verify_sparse(value_or_empty, index, proof, root, depth=4):
    cur = value_or_empty
    pos = index
    for sib in proof:
        if pos % 2 == 0:
            cur = H(cur + sib)
        else:
            cur = H(sib + cur)
        pos //= 2
    return cur == root

# Uso: chave no índice 5 presente, índice 3 ausente
leaves = {5: H("tx5")}
root = sparse_root(leaves, depth=3)
proof_present = sparse_proof(leaves, 5, depth=3)
proof_absent  = sparse_proof(leaves, 3, depth=3)
print("presente:", verify_sparse(H("tx5"), 5, proof_present, root, depth=3))
print("ausente:", verify_sparse(EMPTY, 3, proof_absent, root, depth=3))
```

### Vantagens da Sparse

- **Provas de não-inclusão baratas**: basta mostrar que a posição contém o hash vazio.
- Ideal para grandes espaços de chaves (ex.: contas Ethereum, sparse Merkle trees de estado).

---

## Resumo comparativo

| Aspeto | Dense | Sparse |
|---|---|---|
| Estrutura | Apenas dados existentes | Posição fixa para cada chave |
| Prova de inclusão | Eficiente | Eficiente |
| Prova de não-inclusão | Cara (revelar tudo) | Barata (hash vazio) |
| Uso típico | Blocos de transações | Estado de contas / mapas grandes |