# Desafio: Dijkstra: Shortest Reach 2


##  Dados do Desafio:

- **Link do Desafio**: [Dijkstra: Shortest Reach 2](https://www.hackerrank.com/challenges/dijkstrashortreach/problem)
- **Tema:** Graph Teory
- **Nível:** Difícil | Expert
- **Língaguem:** Python

**Figura 1:** Dados do desafio.

![image](https://github.com/user-attachments/assets/6e27303b-119f-47a6-812b-b4ccee595159)



## Enunciado

![image](https://github.com/user-attachments/assets/a2b62c33-86f2-419e-bc8f-6f3d712bf7bf)

![image](https://github.com/user-attachments/assets/eb7ecd03-6a4a-4505-a863-24c9ba0daf75)

**Figura 2**: Enuciado do desafio.


## Compreendendo o problema:


### Objetivo: 
O objetivo deste problema é calcular as distâncias mínimas de um nó inicial S até todos os outros nós em um grafo ponderado. Se um nó não for alcançável a partir do nó inicial, a distância será -1.

### Entradas:

- T: O número de casos de teste.
- N: O número de nós no grafo.
- M: O número de arestas no grafo.
- Arestas: Cada aresta é representada por três valores [u, v, w], indicando que existe uma aresta entre os nós u e v com peso w.
- S: O nó inicial de onde as distâncias serão calculadas.

### Saída:

Para cada caso de teste, o algoritmo retorna as distâncias mínimas de todos os nós a partir de S, excluindo o nó inicial. Se algum nó for inacessível, a distância será -1.


## Estratégia de solução:

1. Construção do Grafo (Lista de Adjacência):

O grafo é representado como um dicionário de listas de adjacência. Para cada nó, armazenamos suas arestas adjacentes e os pesos dessas arestas.

O grafo é não direcionado, então para cada aresta entre u e v com peso w, adicionamos v a u e u a v com o mesmo peso.

2. Algoritmo de Dijkstra:

O algoritmo de Dijkstra calcula as distâncias mínimas a partir de um nó inicial.

Usamos uma fila de prioridade (min-heap) para sempre escolher o nó com a menor distância conhecida.

Para cada vizinho de um nó, verificamos se a distância para ele pode ser reduzida. Se sim, atualizamos a distância e colocamos o vizinho na fila de prioridade.

3. Fila de Prioridade (Min-Heap):

A fila de prioridade (H) armazena pares de valores, onde o primeiro valor é a distância acumulada até o nó e o segundo é o nó.

A fila permite que sempre processemos o nó com a menor distância.

4. Manutenção da Distância:

Mantemos um dicionário D para armazenar as distâncias mínimas de cada nó a partir do nó inicial S. Inicialmente, todas as distâncias são definidas como infinito (float('inf')), exceto para o nó inicial S, que tem distância 0.

### Código em Python
```python

from collections import defaultdict
import heapq

def dijkstra(S, N, G):
    D = {}  # Dicionário para armazenar as distâncias
    H = [(0, S)]  # Fila de prioridade, iniciando com o nó S e distância 0

    # Inicializa todas as distâncias como infinito
    for n in range(1, N + 1):
        D[n] = float('inf')

    D[S] = 0  

   
    while H:
        t = heapq.heappop(H)  # Retira o nó com a menor distância
        for h in G[t[1]]:  # Explora os vizinhos do nó
            # Atualiza a distância se a nova distância for menor
            if D[h[0]] > D[t[1]] + h[1]:
                D[h[0]] = D[t[1]] + h[1]
                
                # Remove o nó da fila caso ele já esteja e precisa ser atualizado
                if (h[1], h[0]) in H:
                    H.remove((h[1], h[0]))
                    heapq.heapify(H)
                # Adiciona o vizinho na fila de prioridade com a nova distância
                heapq.heappush(H, (D[h[0]], h[0]))

    return D  # Retorna o dicionário de distâncias mínimas

```

## Submissões

![image](https://github.com/user-attachments/assets/f6295872-052a-41a7-b3af-783814b59e26)



**Figura 3:** Mostrar que o código foi aceito.

## Histórico de versão

| Versão | Data | Descrição | Autora | Revisora |
| ------ | ---- | --------- | ------ | -------- |
|1.0 |08/05/2025| Criação do documento |Mylena Angélica | Larissa Stéfane | 
