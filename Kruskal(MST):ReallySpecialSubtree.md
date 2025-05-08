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
