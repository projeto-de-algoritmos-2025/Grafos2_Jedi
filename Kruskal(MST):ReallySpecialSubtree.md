# Desafio: Kruskal (MST): Really Special Subtree

## Dados do Desafio:

- **Link do Desafio**: [Kruskal (MST): Really Special Subtree](https://www.hackerrank.com/challenges/kruskalmstrsub/problem)
- **Tema:** Graph Teory
- **Nível:** Médio
- **Língaguem:** Python

**Figura 1:** Dados do desafio.

<img src="" alt="Descrição" width="400"/>


## Enunciado

**Figura 2**: Enuciado do desafio.

<img src="" alt="Descrição" width="1000"/>


## Passo a Passo da Resolução:


### 1. Compreensão do Prolema:

**Descrição:** Encontrar um subgrafo dentro de um grafo de entrada dado. Assim, o "Really Special SubTree" é definido como um subgrafo que contém todos os nós do grafo original, forma uma árvore ( que não tem ciclos e com um caminho único entre quaisquer dois nós) e que possui o peso total das arestas minimizado (ou seja, é uma Árvore Geradora Mínima - AGM). Além disso, a regra específica para construir essa AGM é processar as arestas em ordem crescente de peso e adicioná-las se não formarem um ciclo. O objetivo final é retornar a soma total dos pesos das arestas que compõem essa AGM.

**Observação:** Uma regra de desempate adicional é fornecida para arestas de mesmo peso: priorizar a aresta cuja soma dos identificadores dos seus nós (u+v) seja a menor.

**Entradas:** 

- **g_nodes**: Um inteiro representando o número de nós no grafo (N). Os nós são numerados de 1 a N.
- **g_from:** Um array de inteiros, onde g_from[i] é o nó de início da i-ésima aresta.
- **g_to:** Um array de inteiros, onde g_to[i] é o nó de fim da i-ésima aresta. **Observação:** Como o grafo é não direcionado, a aresta (g_from[i], g_to[i]) é a mesma que (g_to[i], g_from[i]).
- **g_weight:** Um array de inteiros, onde g_weight[i] é o peso da i-ésima aresta que conecta g_from[i] e g_to[i]. 

#### Exemplo de entrada: 

- 4 6  → o 4 é o **g_nodes** no problema, representa a quantidade total de nós (pontos) no grafo. O 6 representa a quantidade total de arestas (conexões) no grafo.
- 1 2 5  → Uma conexão entre o nó 1 e o nó 2, com peso 5.
- 1 3 3  → Uma conexão entre o nó 1 e o nó 3, com peso 3.
- 4 1 6  → Uma conexão entre o nó 4 e o nó 1, com peso 6.
- 2 4 7  → Uma conexão entre o nó 2 e o nó 4, com peso 7.
- 3 2 4  → Uma conexão entre o nó 3 e o nó 2, com peso 4.
- 3 4 5  → Uma conexão entre o nó 3 e o nó 4, com peso 5.


### 2. Idea de Como Resolver


#### Pensamento

1. Coletar todas as arestas do grafo com seus respectivos nós (u, v) e pesos (w).
2. Classificar as arestas. O critério principal é colocar o peso (w) em ordem crescente e , como regra de desempate, para pesos iguais, usar a soma dos nós (u+v) em ordem crescente.
3. Inicializar a estrutura Union-Find (Conjuntos Disjuntos) para N nós, onde cada nó começa em seu próprio conjunto isolado.
4. Percorrer as arestas uma a uma na ordem que foram classificadas.
5. Para cada aresta (w, u, v): usar a operação find da Union-Find para descobrir a qual componente cada nó (u e v) pertence.
6. Adição Condicional: Se os nós u e v estiverem em componentes diferentes (não há ciclo), adicionar a aresta: Somar seu peso (w) ao peso total acumulado da AGM ou usar a operação union(u, v) da Union-Find para unir os componentes onde u e v estavam.
7. Se os nós u e v já estiverem no mesmo componente, a aresta forma um ciclo com as arestas já escolhidas. Assim, ignorar esta aresta.


### 3. Criação de um psudo código com base no [slide 15 de Grafos 2](https://aprender3.unb.br/pluginfile.php/3112862/mod_resource/content/1/04-mstreview.pdf) 


      Função EncontrarPesoAGMEspecial(N, Arestas):
           1. Preparação e Ordenação das Arestas
           Classifica as arestas. Critério primário: peso (crescente).
           Critério secundário (desempate): soma dos nós (u + v) (crescente).
           Classificar Arestas com base em (peso ASC, (nó_u + nó_v) ASC)
      
          2. Inicialização da Estrutura Union-Find
          Cria N conjuntos disjuntos, um para cada nó.
          // Inicialmente, cada nó é pai de si mesmo.
          Para cada nó `i` de 1 a N:
              Definir pai[i] = i
              Definir tamanho_conjunto[i] = 1 
      
          // Função auxiliar para encontrar o representante (raiz) do conjunto do nó i
          Função Encontrar(i):
              Se pai[i] == i:
                  Retornar i
              Retornar pai[i] = Encontrar(pai[i]) // Compressão de caminho
      
          Função Unir(i, j):
              raiz_i = Encontrar(i)
              raiz_j = Encontrar(j)
              Se raiz_i != raiz_j:
                  // Unir o conjunto menor ao maior
                  Se tamanho_conjunto[raiz_i] < tamanho_conjunto[raiz_j]:
                      Trocar(raiz_i, raiz_j) // Pseudocódigo simples, na implementação faz a troca dos índices
                  pai[raiz_j] = raiz_i
                  tamanho_conjunto[raiz_i] += tamanho_conjunto[raiz_j]
                  Retornar Verdadeiro // Indica que a união ocorreu (aresta adicionada)
              Retornar Falso // Indica que os nós já estavam no mesmo conjunto (ciclo)
      
          // 3. Construção da AGM
          PesoTotalAGM = 0
          ArestasAdicionadas = 0 // Contador para saber quando a AGM está completa (N-1 arestas)
      
        
          // Verifica se os nós da aresta estão em componentes diferentes
           Se Encontrar(nó_u) != Encontrar(nó_v):
           // Se estiverem em componentes diferentes, adicionar a aresta à AGM
           // A função Unir já retorna Verdadeiro se a união ocorreu
               Se Unir(nó_u, nó_v):
                  PesoTotalAGM = PesoTotalAGM + peso
                  ArestasAdicionadas = ArestasAdicionadas + 1
      
                  Se ArestasAdicionadas == N - 1:
                      Quebrar loop // Sair do loop principal
      
          // 4. Resultado
          Retornar PesoTotalAGM
      


### 4. Criação do Código em Python

```#!/bin/python3

import math
import os
import random
import re
import sys

# Implementacao da estrutura Union-Find (Conjuntos Disjuntos)
# Com otimizacoes: compressao de caminho e uniao por tamanho (Union by Size)
class UnionFind:
    def __init__(self, n):
        # Inicializa cada no como seu proprio pai e o tamanho do conjunto como 1
        self.parent = list(range(n + 1)) # Nos sao de 1 a n
        self.size = [1] * (n + 1)

    # Encontra o representante (raiz) do conjunto do no i
    def find(self, i):
        if self.parent[i] == i:
            return i
        # Compressao de caminho: torna o pai do no o representante da raiz
        self.parent[i] = self.find(self.parent[i])
        return self.parent[i]

    # Une os conjuntos que contem os nos i e j
    # Retorna True se a uniao ocorreu (i.e., i e j estavam em conjuntos diferentes)
    # Retorna False se i e j ja estavam no mesmo conjunto (i.e., aresta forma ciclo)
    def union(self, i, j):
        root_i = self.find(i)
        root_j = self.find(j)

        if root_i != root_j:

            if self.size[root_i] < self.size[root_j]:
                root_i, root_j = root_j, root_i 

            self.parent[root_j] = root_i
            self.size[root_i] += self.size[root_j]
            return True 
        return False 



def kruskals(g_nodes, g_from, g_to, g_weight):


    # 1. Preparacao das Arestas
    # Cria uma lista de arestas como tuplas (peso, no_u, no_v)
    edges = []
    for i in range(len(g_from)):
        edges.append((g_weight[i], g_from[i], g_to[i]))

    # 2. Ordenacao das Arestas
    sorted_edges = sorted(edges, key=lambda item: (item[0], item[1] + item[2]))

    # 3. Inicializacao da Estrutura Union-Find
    uf = UnionFind(g_nodes)

    # 4. Construcao da AGM
    total_weight = 0
    edges_added_count = 0 # Contador para saber quando a AGM esta completa (N-1 arestas)

    # Itera sobre as arestas ja classificadas
    for weight, u, v in sorted_edges:
        # Verifica se os nos da aresta estao em componentes diferentes
        if uf.find(u) != uf.find(v):
            if uf.union(u, v):
                total_weight += weight
                edges_added_count += 1

            
            if edges_added_count == g_nodes - 1:
                break 

    return total_weight

if __name__ == '__main__':
    try:
        fptr = open(os.environ['OUTPUT_PATH'], 'w')
    except KeyError:

        fptr = sys.stdout

    # Le a primeira linha: numero de nos (g_nodes) e numero de arestas (g_edges)
    first_multiple_input = input().rstrip().split()
    g_nodes = int(first_multiple_input[0])
    g_edges = int(first_multiple_input[1])

    g_from = [0] * g_edges
    g_to = [0] * g_edges
    g_weight = [0] * g_edges


    for i in range(g_edges):
        edge_item = input().split()
        g_from[i] = int(edge_item[0])
        g_to[i] = int(edge_item[1])
        g_weight[i] = int(edge_item[2])


    res = kruskals(g_nodes, g_from, g_to, g_weight)


    fptr.write(str(res) + '\n')

    if fptr != sys.stdout:
        fptr.close()
```

## Resultado Final

**Figura 3:** Mostrar que o código foi aceito.

<img src="" alt="Descrição" width="800"/>


## Submissões

**Figura 4:** Mostrar as submissões enquanto tenatava resolver.

<img src="" alt="Descrição" width="800"/>

## Histórico de versão

| Versão | Data | Descrição | Autora | Revisora |
| ------ | ---- | --------- | ------ | -------- |
|1.0 |07/05/2025|Criação do Documento|Larissa Stéfane | Mylena Angélica |
|1.1 |07/05/2025|Adição do pseudoCódigo|Larissa Stéfane | Mylena Angélica |
|1.2 |07/05/2025|Adição do Código|Larissa Stéfane | Mylena Angélica |
