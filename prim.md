# Desafio: Prim's (MST) : Special Subtree


##  Dados do Desafio:

- **Link do Desafio**: [Graph Theory: Prim's (MST)- Special Subtree](https://www.hackerrank.com/challenges/primsmstsub/problem?isFullScreen=true)
- **Tema:** Graph Teory
- **Nível:** Médio
- **Língaguem:** Python

**Figura 1:** Dados do desafio.

![image](https://github.com/user-attachments/assets/4e3db3f5-0766-4173-be18-50f3ecdeda6b)


## Enunciado


![image](https://github.com/user-attachments/assets/f5c2d2e5-96fb-4da0-a6aa-cf4f60566dcb)
![image](https://github.com/user-attachments/assets/bcc0c171-5196-4d40-a5ee-07d342be36ff)
**Figura 2**: Enuciado do desafio.


## Compreendendo o problema:


### Objetivo: 
Encontrar a árvore geradora mínima (MST) em um grafo não direcionado, onde o objetivo é conectar todos os nós com a menor soma de pesos nas arestas.

### Entradas:

- n: Número de nós no grafo.

- edges: Uma lista de arestas, onde cada aresta é representada por três números [u, v, w], significando que existe uma aresta entre os nós u e v com peso w.

- start: O nó inicial para o algoritmo de Prim.

### Saída:

O peso mínimo para conectar todos os nós na árvore geradora mínima.

### Propriedades do problema:

- A subárvore (ou subgrafo) deve conter todos os nós.

- A soma das arestas da árvore geradora mínima deve ser a menor possível.

- Deve haver exatamente um caminho entre qualquer par de nós na subárvore.

## Estratégia de solução:

1. Construção do Grafo:
O grafo será representado por uma lista de adjacência. Cada nó terá uma lista de suas arestas conectadas, com o peso da aresta.

2. Fila de Prioridade (Min-Heap):
A fila de prioridade pq é usada para sempre escolher a aresta de menor peso. Ela começa com o nó inicial, com custo 0.

3. Algoritmo de Prim:
O algoritmo de Prim começa de um nó específico (start) e seleciona a aresta de menor peso que conecta um nó já na árvore a um nó fora da árvore. Este processo continua até todos os nós serem incluídos na árvore geradora mínima (MST).


### Código em Python
```python

import math
import os
import random
import re
import sys
import heapq

#
# TRECHO DE CÓDIGO implementado
#

def prims(n, edges, start):
# Lista de adjacência    
    adj = {i: [] for i in range(1, n + 1)}
    for u, v, w in edges:
        adj[u].append((v, w))
        adj[v].append((u, w))
    
#  `min_cost` que armazenará o custo mínimo para conectar cada nó à mst  
    min_cost = {i: float('inf') for i in range(1, n + 1)}
    min_cost[start] = 0

 # Fila de prioridade para sempre escolher o nó com o menor custo. 
    pq = [(0, start)] 
    

    in_mst = set()
    
    total_weight = 0
# Laço principal que vai continuar enquanto houver nós na fila de prioridade (pq).    
    while pq:
        cost, node = heapq.heappop(pq)
        
        if node in in_mst:
            continue
        
      
        in_mst.add(node)
        total_weight += cost
        
      
        for neighbor, weight in adj[node]:
            if neighbor not in in_mst and weight < min_cost[neighbor]:
                min_cost[neighbor] = weight
                heapq.heappush(pq, (weight, neighbor))
    
    return total_weight

# MAIN do exercício
 if __name__ == '__main__':
    fptr = open(os.environ['OUTPUT_PATH'], 'w')

    first_multiple_input = input().rstrip().split()

    n = int(first_multiple_input[0])

    m = int(first_multiple_input[1])

    edges = []

    for _ in range(m):
        edges.append(list(map(int, input().rstrip().split())))

    start = int(input().strip())

    result = prims(n, edges, start)

    fptr.write(str(result) + '\n')

    fptr.close()

```

## Submissões


![image](https://github.com/user-attachments/assets/77bffea7-69bd-41ed-affa-82611adc4088)

**Figura 3:** Mostrar que o código foi aceito.

## Histórico de versão

| Versão | Data | Descrição | Autora | Revisora |
| ------ | ---- | --------- | ------ | -------- |
|1.0 |04/05/2025| Criação do documento |Mylena Angélica | Larissa Stéfane | 

