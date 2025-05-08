# Desafio: Minimum MST Graph

## Dados do Desafio:

- **Link do Desafio**: [Minimum MST Graph](https://www.hackerrank.com/challenges/minimum-mst-graph/submissions/2)
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

**Descrição:** 

**Observação:** 

**Entradas:** 

#### Exemplo de entrada: 



### 2. Idea de Como Resolver


#### Pensamento


### 3. Criação de um pseudo código 



### 4. Criação do Código em Python

```#!/bin/python3

import math
import os
import random
import re
import sys
from typing import List

# Algoritmo de Prim
# Nota: Esta funcao contem logica dummy e nao implementa o algoritmo real de Prim.
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
# Nota: Esta funcao contem logica dummy e nao implementa o algoritmo real de Kruskal.
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

    # Chama as funcoes dummy de Prim e Kruskal (mantidas conforme solicitado)
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

<img src="" alt="Descrição" width="800"/>


## Submissões

**Figura 4:** Mostrar as submissões enquanto tenatava resolver.

<img src="" alt="Descrição" width="800"/>

## Histórico de versão

| Versão | Data | Descrição | Autora | Revisora |
| ------ | ---- | --------- | ------ | -------- |
|1.0 |07/05/2025|Criação do Documento|Larissa Stéfane | Mylena Angélica |
