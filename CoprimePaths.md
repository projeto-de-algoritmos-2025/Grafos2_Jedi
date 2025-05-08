# Desafio: Coprime Paths

## Dados do Desafio:

- **Link do Desafio**: [Coprime Paths](https://www.hackerrank.com/challenges/coprime-paths/problem)
- **Tema:** Graph Teory
- **Nível:** Difícil
- **Língaguem:** Python

**Figura 1:** Dados do desafio.

<img src="" alt="Descrição" width="400"/>


## Enunciado

**Figura 2**: Enuciado do desafio.

<img src="" alt="Descrição" width="1000"/>


## Passo a Passo da Resolução:


### 1. Compreensão do Prolema:

**Descrição:** O problema "Coprime Paths" apresenta um grafo não direcionado e conectado. COm isso, para cada consulta fornecida, que consiste em um par de nós (u e v), deve-se encontrar o caminho mais curto entre eles. Uma vez identificado esse caminho de menor comprimento, a tarefa é contar quantos pares ordenados de nós (a, b) existem sobre este caminho (onde a aparece antes ou é o mesmo nó que b na sequência do caminho) tal que o valor associado ao nó a (V a) seja um divisor do valor associado ao nó b (V b). O resultado para cada consulta é o número total desses pares divisíveis encontrados no caminho mais curto. Assim, vai se utilizar o **Dijkstra**.

Observações:

 - O grafo de entrada é garantido como sendo não direcionado e conectado.
 - Cada nó no grafo tem um valor inteiro associado (V i).

**Entradas:** 

A entrada é fornecida em blocos:

1. A primeira linha contém dois inteiros: o número de nós (n) e o número de arestas (m).
2. A segunda linha contém n inteiros, que são os valores (V i) para cada nó, do nó 1 ao nó n.
3. As próximas m linhas descrevem as arestas do grafo. Cada linha tem dois inteiros (u e v) o que indica uma aresta entre esses nós.
4. As linhas restantes descrevem as consultas. Cada linha de consulta tem dois inteiros (u e v) a qual representa os nós de início e fim para os quais o caminho mais curto deve ser encontrado e analisado.

#### Exemplo de entrada: 

- 6 5 → 6 representa o número total de nós (ou vértices) no grafo e 5 representa o número total de arestas (ou conexões) no grafo.
- 3 2 4 1 6 5 → Contém n (que é 6, do número anterior) números inteiros separados por espaço. Cada número representa o valor de um nó na ordem. Por exemplo, 3 é o valor do nó 1 (V 1).
- 1 2 → Existe uma aresta entre o nó 1 e o nó 2.
- 1 3 → Existe uma aresta entre o nó 1 e o nó 3.
- 2 4 → Existe uma aresta entre o nó 2 e o nó 4.
- 2 5 → Existe uma aresta entre o nó 2 e o nó 5.
- 3 6 → Existe uma aresta entre o nó 3 e o nó 6.
- 4 6 → Primeira consulta, encontre o caminho mais curto entre 4 e 6.
- 5 6 → Segunda consulta, encontre o caminho mais curto entre 5 e 6.
- 1 1 → Terceira consulta, encontre o caminho mais curto entre 1 e 1.
- 1 6 → Quarta consulta, encontre o caminho mais curto entre 1 e 6.
- 6 1 → Quinta consulta, encontre o caminho mais curto entre 6 e 1.

### 2. Idea de Como Resolver

#### Pensamento


### 3. Criação de um psudo código


### 4. Criação do Código em Python


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
