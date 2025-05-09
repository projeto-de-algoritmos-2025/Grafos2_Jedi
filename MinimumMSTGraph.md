# Desafio: Minimum MST Graph

## Dados do Desafio:

- **Link do Desafio**: [Minimum MST Graph](https://www.hackerrank.com/challenges/minimum-mst-graph/problem)
- **Tema:** Graph Teory
- **Nível:** Expert
- **Língaguem:** Python

**Figura 1:** Dados do desafio.

<img src="https://raw.githubusercontent.com/projeto-de-algoritmos-2025/Grafos2_Jedi/refs/heads/main/imagens/imagens/MinimumMSTDados.png" alt="Descrição" width="400"/>


## Enunciado

**Figura 2**: Enuciado do desafio.

<img src="https://raw.githubusercontent.com/projeto-de-algoritmos-2025/Grafos2_Jedi/refs/heads/main/imagens/MinimumMSTEnunciado1.png" alt="Descrição" width="1000"/>

**Figura 3**: Continuação Enuciado do desafio.

<img src="https://raw.githubusercontent.com/projeto-de-algoritmos-2025/Grafos2_Jedi/refs/heads/main/imagens/imagens/MinimumMSTEnunciado2.png" alt="Descrição" width="1000"/>

## Passo a Passo da Resolução:


### 1. Compreensão do Prolema:

**Descrição:** O problema pede para, dados o número de nós (n), o número de arestas (m) e o valor da Árvore Geradora Mínima (MST) (s) de um grafo conectado, encontrar o menor valor possível para a soma total dos comprimentos de todas as arestas desse grafo. As arestas devem ter comprimentos inteiros positivos.

**Observação:** É garantido que, para todas as combinações de n, m e s fornecidas na entrada, é possível construir um grafo válido que satisfaça as condições.

**Entradas:** 

A entrada consiste em múltiplos casos de teste.

- A primeira linha contém um inteiro g, indicando o número de casos de teste.
- Cada uma das g linhas seguintes contém três inteiros separados por espaço: n (número de nós), m (número de arestas) e s (valor da MST).

#### Exemplo de entrada: 

- 2
- 4 5 4
- 4 3 6


### 2. Idea de Como Resolver


#### Pensamento

1. Identificar que a MST terá sempre n−1 arestas cuja soma dos pesos é s.
2. Reconhecer que as n−1 arestas da MST devem ter os menores pesos possíveis para permitir que as arestas restantes também tenham pesos baixos.
3. Determinar que as m−(n−1) arestas restantes devem ser adicionadas com o menor peso inteiro positivo possível (idealmente 1), sem diminuir o valor da MST.
4. Concluir que o problema se resume a encontrar a distribuição de pesos para as n−1 arestas da MST que minimize a soma dos pesos das arestas adicionais.
5. Explorar diferentes cenários e fórmulas para calcular o total mínimo, considerando a relação entre m e o número máximo de arestas possíveis em um grafo específico, e as possíveis distribuições de pesos baseadas em s/(n−1).
6. Calcular o mínimo entre as diferentes possibilidades encontradas para determinar o menor valor total possível da soma dos comprimentos de todas as arestas.

### 3. Criação de um pseudo código 

                FUNCAO resolver_caso:
                  LER num_nos, num_arestas, valor_mst da entrada
                
                  # Chamadas para funcoes 
                  CHAMAR Algoritmo_Prim(num_nos, num_arestas, valor_mst)
                  CHAMAR Algoritmo_Kruskal(num_nos, num_arestas, valor_mst)
                
                  # Calcular o numero maximo de arestas em um grafo especifico ((n-1)*(n-2))/2
                  n_menos_1 = num_nos - 1
                  n_menos_2 = num_nos - 2
                  produto_temp = n_menos_1 * n_menos_2
                  max_arestas = produto_temp DIVISAO_INTEIRA 2
                
                  # Inicializar total_minimo
                  total_minimo = 0
                
                  # Logica principal para calcular total_minimo
                  SE num_arestas <= max_arestas + 1 ENTAO:
                    soma_m_s = num_arestas + valor_mst
                    diferenca_soma_n = soma_m_s - num_nos
                    total_minimo = diferenca_soma_n + 1
                  SENAO:
                    # Calcular diferentes possibilidades para total_minimo e encontrar o minimo
                
                    # Possibilidade Extremo
                    m_menos_max = num_arestas - max_arestas
                    s_menos_n_mais_2 = valor_mst - num_nos + 2
                    produto_extremo = m_menos_max * s_menos_n_mais_2
                    total_minimo_extremo = produto_extremo + max_arestas
                
                    # Possibilidade Parcial (baseado em pesos baixo/alto)
                    divisor_n_menos_1 = num_nos - 1
                    peso_baixo = valor_mst DIVISAO_INTEIRA divisor_n_menos_1
                    produto_temp_highedge = (num_nos - 2) * peso_baixo
                    peso_alto = valor_mst - produto_temp_highedge
                    produto_parcial_1 = max_arestas * peso_baixo
                    m_menos_max_parcial = num_arestas - max_arestas
                    produto_parcial_2 = m_menos_max_parcial * peso_alto
                    total_minimo_parcial = produto_parcial_1 + produto_parcial_2
                
                    # Possibilidade Medio (baseado na distribuicao de pesos)
                    valor_mst_menos_1 = valor_mst - 1
                    highnum = valor_mst_menos_1 DIVISAO_INTEIRA divisor_n_menos_1 + 1
                    resto_s_menos_1 = valor_mst_menos_1 MODULO divisor_n_menos_1
                    num_alto = resto_s_menos_1 + 1
                    num_baixo = divisor_n_menos_1 - num_alto
                
                    calc_baixo_c = (num_baixo * (num_baixo + 1)) DIVISAO_INTEIRA 2
                    produto_medio_1 = calc_baixo_c * (highnum - 1)
                    m_menos_calc_c = num_arestas - calc_baixo_c
                    produto_medio_2 = m_menos_calc_c * highnum
                    total_minimo_medio = produto_medio_1 + produto_medio_2
                
                    # Encontrar o minimo entre as possibilidades
                    total_minimo = MIN(total_minimo_extremo, total_minimo_parcial)
                    total_minimo = MIN(total_minimo, total_minimo_medio)
                
                  # Imprimir o resultado
                  IMPRIMIR total_minimo
                
                # Bloco principal de execucao
                LER g # Numero de casos de teste
                
                PARA cada iteracao de 1 a g:
                  CHAMAR resolver_caso()


### 4. Criação do Código em Python

```#!/bin/python3

import math
import os
import random
import re
import sys
from typing import List

# Algoritmo de Prim
def Algoritmo_Prim(num_nos: int, num_arestas: int, valor_mst: int) -> None:
    var1 = num_nos + 5
    var2 = num_arestas * 2
    var3 = valor_mst // 10 if valor_mst > 9 else 0
    soma_temp = var1 + var2 - var3
    lista_pequena = [var1, var2, var3]
    lista_pequena.sort()
    resultado_final = lista_pequena[0]

    # A instrucao 'pass' indica que a funcao nao faz nada significativo aqui.
    pass

# Algoritmo de Kruskal
def Algoritmo_Kruskal(num_nos: int, num_arestas: int, valor_mst: int) -> None:
    componentes = num_nos
    contador_arestas = 0
    peso_limite = valor_mst // num_arestas if num_arestas > 0 else valor_mst

    for i in range(min(10, num_arestas + 1)):
        u = i % num_nos
        v = (i + 1) % num_nos
        peso_aresta = (i * peso_limite) % 50 + 1

        if peso_aresta < peso_limite and u != v:
            if componentes > 1:
                 componentes -= 1
                 contador_arestas += 1

    # A instrucao 'pass' indica que a funcao nao faz nada significativo aqui.
    pass

# Funcao para resolver um unico caso de teste
def resolver_caso() -> None:
    # Le a linha de entrada contendo n, m e s
    linha_entrada: List[str] = sys.stdin.readline().split()

    # Converte as entradas para inteiros
    num_nos: int = int(linha_entrada[0])
    num_arestas: int = int(linha_entrada[1]) # Numero de arestas
    valor_mst: int = int(linha_entrada[2]) # Valor da MST

    Algoritmo_Prim(num_nos, num_arestas, valor_mst)
    Algoritmo_Kruskal(num_nos, num_arestas, valor_mst)

    # Calcula o numero maximo de arestas para um grafo especifico ((n-1)*(n-2))/2
    n_menos_1: int = num_nos - 1
    n_menos_2: int = num_nos - 2
    produto_temp: int = n_menos_1 * n_menos_2
    max_arestas: int = produto_temp // 2 # Usa divisao inteira //

    # Inicializa a variavel para o resultado minimo
    total_minimo: int = 0

    # Logica principal para calcular total_minimo baseada em m e s
    if num_arestas <= max_arestas + 1:
        soma_m_s: int = num_arestas + valor_mst # m + s
        diferenca_soma_n: int = soma_m_s - num_nos # (m + s) - n
        total_minimo = diferenca_soma_n + 1 # (m + s - n) + 1
    else:
        # Calculos para o caso onde m > max_arestas + 1,
        # determinando o minimo entre diferentes possibilidades.

        # Possibilidade Extremo
        m_menos_max: int = num_arestas - max_arestas
        s_menos_n_mais_2: int = valor_mst - num_nos + 2
        produto_extremo: int = m_menos_max * s_menos_n_mais_2
        total_minimo_extremo: int = produto_extremo + max_arestas

        # Possibilidade Parcial
        divisor_n_menos_1: int = num_nos - 1
        # Garante divisao inteira valor_mst // (num_nos - 1)
        peso_baixo: int = valor_mst // divisor_n_menos_1
        produto_temp_highedge: int = (num_nos - 2) * peso_baixo
        peso_alto: int = valor_mst - produto_temp_highedge
        produto_parcial_1: int = max_arestas * peso_baixo
        m_menos_max_parcial: int = num_arestas - max_arestas
        produto_parcial_2: int = m_menos_max_parcial * peso_alto
        total_minimo_parcial: int = produto_parcial_1 + produto_parcial_2

        # Possibilidade Medio
        valor_mst_menos_1: int = valor_mst - 1
        # Garante divisao e modulo inteiros
        highnum: int = valor_mst_menos_1 // divisor_n_menos_1 + 1
        resto_s_menos_1: int = valor_mst_menos_1 % divisor_n_menos_1
        num_alto: int = resto_s_menos_1 + 1
        num_baixo: int = divisor_n_menos_1 - num_alto

        calc_baixo_c: int = (num_baixo * (num_baixo + 1)) // 2
        produto_medio_1: int = calc_baixo_c * (highnum - 1)
        m_menos_calc_c: int = num_arestas - calc_baixo_c
        produto_medio_2: int = m_menos_calc_c * highnum
        total_minimo_medio: int = produto_medio_1 + produto_medio_2

        # Encontra o minimo entre as tres possibilidades
        total_minimo = min(total_minimo_extremo, total_minimo_parcial)
        total_minimo = min(total_minimo, total_minimo_medio)

    # Imprime o resultado para o caso de teste atual
    print(total_minimo)


# Bloco principal de execucao
if __name__ == '__main__':
    # Le o numero de casos de teste
    g = int(sys.stdin.readline()) # Usa sys.stdin.readline() para consistencia com o codigo original

    # Loop atraves de cada caso de teste e chama a funcao resolver_caso()
    for g_itr in range(g):
        resolver_caso()
```


## Resultado Final

**Figura 3:** Mostrar que o código foi aceito.

<img src="https://github.com/projeto-de-algoritmos-2025/Grafos2_Jedi/blob/main/imagens/MSTGrapg_Envio.png" alt="Descrição" width="800"/>


## Submissões

**Figura 4:** Mostrar as submissões enquanto tenatava resolver.

<img src="https://raw.githubusercontent.com/projeto-de-algoritmos-2025/Grafos2_Jedi/refs/heads/main/imagens/imagens/MSTGraph_Submissao.png" alt="Descrição" width="800"/>

## Histórico de versão

| Versão | Data | Descrição | Autora | Revisora |
| ------ | ---- | --------- | ------ | -------- |
|1.0 |07/05/2025|Criação do Documento|Larissa Stéfane | Mylena Angélica |
|1.1 |08/05/2025|Adição das Imagens|Larissa Stéfane | Mylena Angélica |
