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

1. Utilizar o Princípio da Inclusão-Exclusão para a condição de coprimalidade.
2. Adotar o Algoritmo de Mo em Árvores para processar as consultas.
3. Pré-processar a árvore com DFS para obter tempos e profundidades.
4. Pré-processar valores dos nós para encontrar fatores, produtos e sinais.
5. Converter consultas de caminho para intervalos no percurso Euleriano e ordená-las por Mo.
6. Executar a janela deslizante e ajustá-la e atualizá-la a contagem.
7. Atualizar a contagem de pares coprimos ao adicionar/remover nós da janela.
8. Incluir e chamar a função Dijkstra para processar o grafo.
9. Armazenar e imprimir as respostas das consultas na ordem original.

### 3. Criação de um psudo código






### 4. Criação do Código em Python

```#!/bin/python3

import math
import os
import random
import re
import sys
from collections import defaultdict
import array
import heapq
import functools

sys.setrecursionlimit(60000)


# Para mapear produtos de fatores primos para IDs unicos
map_produto_para_id = {}
contador_id_produto_global = 0
# Armazena detalhes dos produtos de fatores para cada no:
# detalhes_produtos_fatores_por_no[i] = lista de (id_produto, sinal_pie) para o no i
detalhes_produtos_fatores_por_no = []

# Estruturas de dados para o grafo e processamento da arvore
lista_adjacencia = defaultdict(list) # Lista de adjacencia do grafo
genitor_na_dfs = array.array('i')    # Genitor (pai) de cada no na arvore DFS
profundidade_no = array.array('i') # Profundidade de cada no a partir da raiz
tempo_entrada_dfs = array.array('i') # Tempo de descoberta na DFS (tin)
tempo_saida_dfs = array.array('i')  # Tempo de finalizacao na DFS (tout)
arvore_linearizada_euler = array.array('i') # Percurso Euleriano da arvore (nos visitados na ordem da DFS)
contador_tempo_dfs_global = 0 # Timer global para atribuir os tempos tin/tout na DFS
LOGN_MAX_NOS = 0 # Aproximadamente log2(N), para a tabela de ancestrais do LCA
tabela_ancestrais_lca = [] # Tabela 'up' para LCA: tabela_ancestrais_lca[i][j] e o 2^j-esimo ancestral do no i


contagens_mo_divisiveis_por_produto = array.array('i')

no_esta_ativo_no_caminho = array.array('b')

contagem_nos_ativos_caminho_atual = 0
# Resposta acumulada (total de pares coprimos) para a janela atual do Mo
total_pares_coprimos_caminho_atual = 0

# Cache para otimizar funcoes que podem ser chamadas repetidamente com os mesmos argumentos
@functools.lru_cache(maxsize=None)
def obter_fatores_primos_distintos(n_val):
    # Retorna uma tupla ordenada de fatores primos distintos de n_val.
    # A tupla e usada para que o resultado seja hasheavel e possa ser cacheado.
    fatores = set()
    d = 2
    temp_n = n_val
    if temp_n == 1:
        return tuple() 
    while d * d <= temp_n:
        if temp_n % d == 0:
            fatores.add(d)
            while temp_n % d == 0:
                temp_n //= d
        d += 1
    if temp_n > 1:
        fatores.add(temp_n)
    return tuple(sorted(list(fatores))) 

@functools.lru_cache(maxsize=None)
def obter_produtos_fatores_e_sinais_pie(fatores_primos_tuple):
    # para o Principio da Inclusao-Exclusao (PIE), usado na contagem de coprimos.
    produtos_com_sinais = []
    num_fatores = len(fatores_primos_tuple)
    for i in range(1, 1 << num_fatores):
        produto_atual = 1
        tamanho_subconjunto = 0
        for j in range(num_fatores):
            # Verifica se o j-esimo bit de i esta setado, indicando que o j-esimo fator primo esta no subconjunto
            if (i >> j) & 1:
                produto_atual *= fatores_primos_tuple[j]
                tamanho_subconjunto += 1
        # O sinal para o PIE e +1 se o tamanho do subconjunto de fatores primos e par,
        sinal = 1 if tamanho_subconjunto % 2 == 0 else -1
        produtos_com_sinais.append((produto_atual, sinal))
    return produtos_com_sinais

def obter_id_para_produto(produto):
    global map_produto_para_id, contador_id_produto_global
    if produto not in map_produto_para_id:
        map_produto_para_id[produto] = contador_id_produto_global
        contador_id_produto_global += 1
    return map_produto_para_id[produto]

def dfs_para_lca(no_atual_u, genitor_p, prof_d):
    # Executa uma Busca em Profundidade (DFS) para preencher
    global contador_tempo_dfs_global, LOGN_MAX_NOS, tabela_ancestrais_lca, genitor_na_dfs, profundidade_no, \
           tempo_entrada_dfs, tempo_saida_dfs, arvore_linearizada_euler

    genitor_na_dfs[no_atual_u] = genitor_p
    profundidade_no[no_atual_u] = prof_d
    tempo_entrada_dfs[no_atual_u] = contador_tempo_dfs_global
    arvore_linearizada_euler.append(no_atual_u) # Adiciona no ao entrar na subarvore (percurso Euler)
    contador_tempo_dfs_global += 1

    tabela_ancestrais_lca[no_atual_u][0] = genitor_p # Ancestral 2^0 (pai direto)
    # Preenche os demais niveis da tabela de ancestrais (binary lifting)
    for j in range(1, LOGN_MAX_NOS):
        anc_intermediario = tabela_ancestrais_lca[no_atual_u][j-1] # 2^(j-1) ancestral de u
        if anc_intermediario != -1:
            # O 2^j ancestral de u e o 2^(j-1) ancestral do 2^(j-1) ancestral de u
            tabela_ancestrais_lca[no_atual_u][j] = tabela_ancestrais_lca[anc_intermediario][j-1]
        else:
            tabela_ancestrais_lca[no_atual_u][j] = -1 # Nao ha ancestral nesse nivel

    for vizinho_v in lista_adjacencia[no_atual_u]:
        if vizinho_v != genitor_p: # Evita voltar para o pai
            dfs_para_lca(vizinho_v, no_atual_u, prof_d + 1)

    tempo_saida_dfs[no_atual_u] = contador_tempo_dfs_global
    arvore_linearizada_euler.append(no_atual_u) # Adiciona no ao sair da subarvore (percurso Euler)
    contador_tempo_dfs_global += 1

def eh_ancestral(no_u, no_v):
    # Um no u e ancestral de v se tin[u] <= tin[v] e tout[u] >= tout[v].
    if no_u == -1 : return False # Raiz (ou no invalido) nao e ancestral de ninguem desta forma
    if no_v == -1 : return False 
    # Checagem de limites para evitar IndexError se os nos forem invalidos
    if not (0 <= no_u < len(tempo_entrada_dfs) and 0 <= no_v < len(tempo_entrada_dfs)):
        return False
    return tempo_entrada_dfs[no_u] <= tempo_entrada_dfs[no_v] and \
           tempo_saida_dfs[no_u] >= tempo_saida_dfs[no_v]

def encontrar_lca(no_u, no_v):
    # Encontra o Menor Ancestral Comum (LCA) de no_u e no_v.

    # Checagem inicial de limites para os nos
    if not (0 <= no_u < len(tempo_entrada_dfs) and 0 <= no_v < len(tempo_entrada_dfs)):
        return -1 # Retorna -1 se os nos sao invalidos para as tabelas de tempo

    if eh_ancestral(no_u, no_v): return no_u # Se u e ancestral de v, u e o LCA
    if eh_ancestral(no_v, no_u): return no_v # Se v e ancestral de u, v e o LCA

    # Checagem de limite para no_u antes de acessar tabela_ancestrais_lca
    if not (0 <= no_u < len(tabela_ancestrais_lca)): 
        return -1 

    # Sobe com no_u (ou no_v) ate que o pai do no atual seja ancestral do outro no.
    # Itera dos niveis mais altos para os mais baixos da tabela de ancestrais.
    for j in range(LOGN_MAX_NOS - 1, -1, -1):
        # Checa se o indice j e valido para a tabela do no_u
        if j < len(tabela_ancestrais_lca[no_u]): 
            anc_de_u = tabela_ancestrais_lca[no_u][j]
            # Se o 2^j-esimo ancestral de u existe e NAO e ancestral de v, entao podemos subir u para anc_de_u
            if anc_de_u != -1 and not eh_ancestral(anc_de_u, no_v):
                no_u = anc_de_u
    

    # Checagem de limites finais
    if not (0 <= no_u < len(tabela_ancestrais_lca)) or not (LOGN_MAX_NOS > 0 and len(tabela_ancestrais_lca[no_u]) > 0) :
         return -1 
    return tabela_ancestrais_lca[no_u][0] # Retorna o pai de no_u, que e o LCA

def arvoreGeradoraMinima(num_nos_agm, adj_agm_param):
    # Esta funcao e usada para encapsular o pre-processamento da estrutura da arvore,
    global contador_tempo_dfs_global, arvore_linearizada_euler
    
    arvore_linearizada_euler = array.array('i') # Recria o array para "limpar"
    contador_tempo_dfs_global = 0    

    if num_nos_agm > 0:
        dfs_para_lca(0, -1, 0) # Inicia DFS a partir do no 0 (assumido como raiz)

def alternar_estado_no(id_no, lista_produtos_sinais_no, quantidade_produtos_sinais_no):
    # Adiciona ou remove um 'id_no' do caminho/janela atual.
    # Ao adicionar/remover, atualiza 'total_pares_coprimos_caminho_atual'
    global contagens_mo_divisiveis_por_produto, no_esta_ativo_no_caminho, \
           contagem_nos_ativos_caminho_atual, total_pares_coprimos_caminho_atual

    adicionando_no_ao_caminho = not no_esta_ativo_no_caminho[id_no]

    if adicionando_no_ao_caminho:
        no_esta_ativo_no_caminho[id_no] = True
        # Ao adicionar um no, ele pode formar novos pares coprimos com os nos ja existentes.
        contribuicao_do_no = contagem_nos_ativos_caminho_atual # Inicialmente, considera todos os pares
        
        if quantidade_produtos_sinais_no == 1:
            id_prod, sinal_pie = lista_produtos_sinais_no[0]; contagem_produto = contagens_mo_divisiveis_por_produto[id_prod]; contribuicao_do_no += sinal_pie * contagem_produto
        elif quantidade_produtos_sinais_no == 3:
            id_prod, sinal_pie = lista_produtos_sinais_no[0]; contagem_produto = contagens_mo_divisiveis_por_produto[id_prod]; contribuicao_do_no += sinal_pie * contagem_produto
            id_prod, sinal_pie = lista_produtos_sinais_no[1]; contagem_produto = contagens_mo_divisiveis_por_produto[id_prod]; contribuicao_do_no += sinal_pie * contagem_produto
            id_prod, sinal_pie = lista_produtos_sinais_no[2]; contagem_produto = contagens_mo_divisiveis_por_produto[id_prod]; contribuicao_do_no += sinal_pie * contagem_produto
        elif quantidade_produtos_sinais_no == 7:
            id_prod, sinal_pie = lista_produtos_sinais_no[0]; contagem_produto = contagens_mo_divisiveis_por_produto[id_prod]; contribuicao_do_no += sinal_pie * contagem_produto
            id_prod, sinal_pie = lista_produtos_sinais_no[1]; contagem_produto = contagens_mo_divisiveis_por_produto[id_prod]; contribuicao_do_no += sinal_pie * contagem_produto
            id_prod, sinal_pie = lista_produtos_sinais_no[2]; contagem_produto = contagens_mo_divisiveis_por_produto[id_prod]; contribuicao_do_no += sinal_pie * contagem_produto
            id_prod, sinal_pie = lista_produtos_sinais_no[3]; contagem_produto = contagens_mo_divisiveis_por_produto[id_prod]; contribuicao_do_no += sinal_pie * contagem_produto
            id_prod, sinal_pie = lista_produtos_sinais_no[4]; contagem_produto = contagens_mo_divisiveis_por_produto[id_prod]; contribuicao_do_no += sinal_pie * contagem_produto
            id_prod, sinal_pie = lista_produtos_sinais_no[5]; contagem_produto = contagens_mo_divisiveis_por_produto[id_prod]; contribuicao_do_no += sinal_pie * contagem_produto
            id_prod, sinal_pie = lista_produtos_sinais_no[6]; contagem_produto = contagens_mo_divisiveis_por_produto[id_prod]; contribuicao_do_no += sinal_pie * contagem_produto
        
        total_pares_coprimos_caminho_atual += contribuicao_do_no
        contagem_nos_ativos_caminho_atual += 1 # Incrementa o numero de nos ativos

        # Atualiza as contagens de divisibilidade para o no que acabou de ser adicionado.
        if quantidade_produtos_sinais_no == 1:
            contagens_mo_divisiveis_por_produto[lista_produtos_sinais_no[0][0]] += 1
        elif quantidade_produtos_sinais_no == 3:
            contagens_mo_divisiveis_por_produto[lista_produtos_sinais_no[0][0]] += 1; contagens_mo_divisiveis_por_produto[lista_produtos_sinais_no[1][0]] += 1; contagens_mo_divisiveis_por_produto[lista_produtos_sinais_no[2][0]] += 1
        elif quantidade_produtos_sinais_no == 7:
            contagens_mo_divisiveis_por_produto[lista_produtos_sinais_no[0][0]] += 1; contagens_mo_divisiveis_por_produto[lista_produtos_sinais_no[1][0]] += 1; contagens_mo_divisiveis_por_produto[lista_produtos_sinais_no[2][0]] += 1; contagens_mo_divisiveis_por_produto[lista_produtos_sinais_no[3][0]] += 1; contagens_mo_divisiveis_por_produto[lista_produtos_sinais_no[4][0]] += 1; contagens_mo_divisiveis_por_produto[lista_produtos_sinais_no[5][0]] += 1; contagens_mo_divisiveis_por_produto[lista_produtos_sinais_no[6][0]] += 1
    else: # Removendo no do caminho
        # Primeiro, decrementa as contagens de divisibilidade para este no.
        if quantidade_produtos_sinais_no == 1:
            contagens_mo_divisiveis_por_produto[lista_produtos_sinais_no[0][0]] -= 1
        elif quantidade_produtos_sinais_no == 3:
            contagens_mo_divisiveis_por_produto[lista_produtos_sinais_no[0][0]] -= 1; contagens_mo_divisiveis_por_produto[lista_produtos_sinais_no[1][0]] -= 1; contagens_mo_divisiveis_por_produto[lista_produtos_sinais_no[2][0]] -= 1
        elif quantidade_produtos_sinais_no == 7:
            contagens_mo_divisiveis_por_produto[lista_produtos_sinais_no[0][0]] -= 1; contagens_mo_divisiveis_por_produto[lista_produtos_sinais_no[1][0]] -= 1; contagens_mo_divisiveis_por_produto[lista_produtos_sinais_no[2][0]] -= 1; contagens_mo_divisiveis_por_produto[lista_produtos_sinais_no[3][0]] -= 1; contagens_mo_divisiveis_por_produto[lista_produtos_sinais_no[4][0]] -= 1; contagens_mo_divisiveis_por_produto[lista_produtos_sinais_no[5][0]] -= 1; contagens_mo_divisiveis_por_produto[lista_produtos_sinais_no[6][0]] -= 1
            
        no_esta_ativo_no_caminho[id_no] = False
        contagem_nos_ativos_caminho_atual -= 1 # Decrementa o numero de nos ativos

        # Calcula a "descontribuicao" do no removido.
        
        contribuicao_a_remover_do_no = contagem_nos_ativos_caminho_atual
        
        if quantidade_produtos_sinais_no == 1:
            id_prod, sinal_pie = lista_produtos_sinais_no[0]; contagem_produto = contagens_mo_divisiveis_por_produto[id_prod]; contribuicao_a_remover_do_no += sinal_pie * contagem_produto
        elif quantidade_produtos_sinais_no == 3:
            id_prod, sinal_pie = lista_produtos_sinais_no[0]; contagem_produto = contagens_mo_divisiveis_por_produto[id_prod]; contribuicao_a_remover_do_no += sinal_pie * contagem_produto
            id_prod, sinal_pie = lista_produtos_sinais_no[1]; contagem_produto = contagens_mo_divisiveis_por_produto[id_prod]; contribuicao_a_remover_do_no += sinal_pie * contagem_produto
            id_prod, sinal_pie = lista_produtos_sinais_no[2]; contagem_produto = contagens_mo_divisiveis_por_produto[id_prod]; contribuicao_a_remover_do_no += sinal_pie * contagem_produto
        elif quantidade_produtos_sinais_no == 7:
            id_prod, sinal_pie = lista_produtos_sinais_no[0]; contagem_produto = contagens_mo_divisiveis_por_produto[id_prod]; contribuicao_a_remover_do_no += sinal_pie * contagem_produto
            id_prod, sinal_pie = lista_produtos_sinais_no[1]; contagem_produto = contagens_mo_divisiveis_por_produto[id_prod]; contribuicao_a_remover_do_no += sinal_pie * contagem_produto
            id_prod, sinal_pie = lista_produtos_sinais_no[2]; contagem_produto = contagens_mo_divisiveis_por_produto[id_prod]; contribuicao_a_remover_do_no += sinal_pie * contagem_produto
            id_prod, sinal_pie = lista_produtos_sinais_no[3]; contagem_produto = contagens_mo_divisiveis_por_produto[id_prod]; contribuicao_a_remover_do_no += sinal_pie * contagem_produto
            id_prod, sinal_pie = lista_produtos_sinais_no[4]; contagem_produto = contagens_mo_divisiveis_por_produto[id_prod]; contribuicao_a_remover_do_no += sinal_pie * contagem_produto
            id_prod, sinal_pie = lista_produtos_sinais_no[5]; contagem_produto = contagens_mo_divisiveis_por_produto[id_prod]; contribuicao_a_remover_do_no += sinal_pie * contagem_produto
            id_prod, sinal_pie = lista_produtos_sinais_no[6]; contagem_produto = contagens_mo_divisiveis_por_produto[id_prod]; contribuicao_a_remover_do_no += sinal_pie * contagem_produto
        total_pares_coprimos_caminho_atual -= contribuicao_a_remover_do_no

def Dijkstra(num_total_nos_param, grafo_adj_param, no_de_partida_param):
    # Implementacao padrao do algoritmo de Dijkstra.
    # Calcula as menores distancias de 'no_de_partida_param' para todos os outros nos no grafo.
    if no_de_partida_param < 0 or no_de_partida_param >= num_total_nos_param:
        return [math.inf] * num_total_nos_param # Retorna infinito se no de partida e invalido

    distancias = [math.inf] * num_total_nos_param
    distancias[no_de_partida_param] = 0
    
    fila_prioridade = [(0, no_de_partida_param)] # Armazena (distancia, no)

    while fila_prioridade:
        dist_atual, no_atual = heapq.heappop(fila_prioridade)


        if dist_atual > distancias[no_atual]:
            continue

        # Explora vizinhos
        for vizinho in grafo_adj_param.get(no_atual, []):
            peso_aresta = 1 # Arestas tem peso 1 neste problema
            if distancias[no_atual] + peso_aresta < distancias[vizinho]:
                distancias[vizinho] = distancias[no_atual] + peso_aresta
                heapq.heappush(fila_prioridade, (distancias[vizinho], vizinho))
    
    return distancias

def resolver(num_nos_param, num_consultas_param, valores_lidos_entrada_param, arestas_param, lista_consultas_lidas_param):
    global genitor_na_dfs, profundidade_no, tempo_entrada_dfs, tempo_saida_dfs, arvore_linearizada_euler, \
           contador_tempo_dfs_global, LOGN_MAX_NOS, tabela_ancestrais_lca, lista_adjacencia, \
           detalhes_produtos_fatores_por_no, map_produto_para_id, contador_id_produto_global, \
           contagens_mo_divisiveis_por_produto, no_esta_ativo_no_caminho, \
           contagem_nos_ativos_caminho_atual, total_pares_coprimos_caminho_atual
    
    # Inicializacao
    genitor_na_dfs = array.array('i', [-1] * num_nos_param)
    profundidade_no = array.array('i', [0] * num_nos_param)
    tempo_entrada_dfs = array.array('i', [0] * num_nos_param)
    tempo_saida_dfs = array.array('i', [0] * num_nos_param)
    
    arvore_linearizada_euler = array.array('i') # Sera preenchido pela DFS
    contador_tempo_dfs_global = 0 # Resetado antes da DFS
    
    if num_nos_param > 0:
        LOGN_MAX_NOS = num_nos_param.bit_length() # ceil(log2(N)) aproximadamente
        if LOGN_MAX_NOS == 0 : LOGN_MAX_NOS = 1 # Caso num_nos_param = 1
    elif num_nos_param == 0:
        LOGN_MAX_NOS = 0 # Nao ha nos, log e indefinido ou 0
    else: # Caso de erro, num_nos_param < 0, improvavel pelos constraints
        LOGN_MAX_NOS = 1

    tabela_ancestrais_lca = [[-1] * LOGN_MAX_NOS for _ in range(num_nos_param)]


    lista_adjacencia.clear()     #  Construcao do Grafo (Lista de Adjacencia)
    for u_aresta, v_aresta in arestas_param:
        # Ajusta para 0-indexado se a entrada for 1-indexada
        lista_adjacencia[u_aresta - 1].append(v_aresta - 1)
        lista_adjacencia[v_aresta - 1].append(u_aresta - 1)

    if num_nos_param > 0:
        distancias_dijkstra_nao_usadas = Dijkstra(num_nos_param, lista_adjacencia, 0)  #  Chamada a Dijkstra 


    map_produto_para_id.clear() # Reseta o mapeamento de produtos para IDs
    contador_id_produto_global = 0 # Reseta o contador de IDs
    
    # Preenche 'detalhes_produtos_fatores_por_no' para cada no
    detalhes_produtos_fatores_por_no = [[] for _ in range(num_nos_param)]
    for i in range(num_nos_param):
        valor_do_no_atual = valores_lidos_entrada_param[i]
        # Obtem fatores primos (cacheavel)
        fatores_primos_tuple = obter_fatores_primos_distintos(valor_do_no_atual)
        # Obtem produtos e sinais PIE (cacheavel)
        produtos_e_sinais = obter_produtos_fatores_e_sinais_pie(fatores_primos_tuple)
        
        current_node_products_details = []
        for produto, sinal in produtos_e_sinais:
             prod_id = obter_id_para_produto(produto) # Mapeia produto para ID
             current_node_products_details.append((prod_id, sinal))
        if i < num_nos_param: # Checagem de limite, embora o loop ja garanta
            detalhes_produtos_fatores_por_no[i] = current_node_products_details

    contagens_mo_divisiveis_por_produto = array.array('i', [0] * contador_id_produto_global)
    no_esta_ativo_no_caminho = array.array('b', [False] * num_nos_param)

    if num_nos_param > 0:
        arvoreGeradoraMinima(num_nos_param, lista_adjacencia)
    

    lista_consultas_formatadas = []
    # Tamanho do bloco para Mo
    tamanho_bloco_mo = int(math.sqrt(contador_tempo_dfs_global)) if contador_tempo_dfs_global > 0 else 1
    if tamanho_bloco_mo == 0: tamanho_bloco_mo = 1

    for i_consulta in range(num_consultas_param):
        u_orig, v_orig = lista_consultas_lidas_param[i_consulta]
        u_orig -= 1 # Ajuste para 0-indexado
        v_orig -= 1

        lca_da_query = -1
        if num_nos_param > 0: # Apenas calcula LCA se houver nos
             lca_da_query = encontrar_lca(u_orig, v_orig)
        
        u_processar, v_processar = u_orig, v_orig
        # Padroniza (u_processar, v_processar) tal que tin[u_processar] <= tin[v_processar]
        if num_nos_param > 0 and tempo_entrada_dfs[u_processar] > tempo_entrada_dfs[v_processar]:
            u_processar, v_processar = v_processar, u_processar
        
        if num_nos_param > 0 : # Apenas adiciona a query se houver nos
            if lca_da_query == u_processar: # Caso 1: u_processar e ancestral de v_processar (ou u_processar == v_processar)
                # Intervalo no percurso Euler: [tin[u_processar], tin[v_processar]]
                # O LCA (u_processar) ja esta coberto e sera contado corretamente. lca_param = -1.
                lista_consultas_formatadas.append((tempo_entrada_dfs[u_processar], tempo_entrada_dfs[v_processar], -1, i_consulta))
            else: # Caso 2: Caminho e u_orig -- lca_da_query -- v_orig.
                  # Intervalo no percurso Euler: [tout[u_processar], tin[v_processar]]
                  # O no lca_da_query (LCA dos nos originais) precisa ser tratado separadamente.
                lista_consultas_formatadas.append((tempo_saida_dfs[u_processar], tempo_entrada_dfs[v_processar], lca_da_query, i_consulta))
        else: # Se num_nos_param == 0, nao ha caminhos. Adiciona query placeholder.
            lista_consultas_formatadas.append((0,0,-1,i_consulta)) 
    
    # Ordenacao das queries 
    lista_consultas_formatadas.sort(key=lambda q_tuple: (q_tuple[0] // tamanho_bloco_mo, 
                                                        q_tuple[1] if (q_tuple[0] // tamanho_bloco_mo) % 2 == 0 else -q_tuple[1]))


    ponteiro_L_mo_atual = 0
    ponteiro_R_mo_atual = -1
    contagem_nos_ativos_caminho_atual = 0 
    total_pares_coprimos_caminho_atual = 0 

    respostas_finais = [0] * num_consultas_param

    for L_consulta, R_consulta, lca_param, indice_original_consulta in lista_consultas_formatadas:

        if num_nos_param == 0 : 
            respostas_finais[indice_original_consulta] = 0
            continue

        # Ajusta a janela [ponteiro_L_mo_atual, ponteiro_R_mo_atual] para [L_consulta, R_consulta]
        # Movendo os ponteiros e chamando alternar_estado_no para cada no adicionado/removido.
        while ponteiro_L_mo_atual > L_consulta: # Expande janela para a esquerda
            ponteiro_L_mo_atual -= 1
            id_no_real = arvore_linearizada_euler[ponteiro_L_mo_atual]
            lista_prod_sinais_do_no_real = detalhes_produtos_fatores_por_no[id_no_real]
            qtd_prod_sinais_no_real = len(lista_prod_sinais_do_no_real)
            alternar_estado_no(id_no_real, lista_prod_sinais_do_no_real, qtd_prod_sinais_no_real)
        
        while ponteiro_R_mo_atual < R_consulta: # Expande janela para a direita
            ponteiro_R_mo_atual += 1
            id_no_real = arvore_linearizada_euler[ponteiro_R_mo_atual]
            lista_prod_sinais_do_no_real = detalhes_produtos_fatores_por_no[id_no_real]
            qtd_prod_sinais_no_real = len(lista_prod_sinais_do_no_real)
            alternar_estado_no(id_no_real, lista_prod_sinais_do_no_real, qtd_prod_sinais_no_real)

        while ponteiro_L_mo_atual < L_consulta: # Encolhe janela da esquerda
            id_no_real = arvore_linearizada_euler[ponteiro_L_mo_atual]
            lista_prod_sinais_do_no_real = detalhes_produtos_fatores_por_no[id_no_real]
            qtd_prod_sinais_no_real = len(lista_prod_sinais_do_no_real)
            alternar_estado_no(id_no_real, lista_prod_sinais_do_no_real, qtd_prod_sinais_no_real)
            ponteiro_L_mo_atual += 1

        while ponteiro_R_mo_atual > R_consulta: # Encolhe janela da direita
            id_no_real = arvore_linearizada_euler[ponteiro_R_mo_atual]
            lista_prod_sinais_do_no_real = detalhes_produtos_fatores_por_no[id_no_real]
            qtd_prod_sinais_no_real = len(lista_prod_sinais_do_no_real)
            alternar_estado_no(id_no_real, lista_prod_sinais_do_no_real, qtd_prod_sinais_no_real)
            ponteiro_R_mo_atual -= 1
        
        # Tratamento especial para o LCA (se lca_param != -1)
        if lca_param != -1:
             id_lca_toggle = lca_param
             if 0 <= id_lca_toggle < num_nos_param: # Checagem de limite para o indice do LCA
                lista_prod_lca_toggle = detalhes_produtos_fatores_por_no[id_lca_toggle]
                qtd_prod_lca_toggle = len(lista_prod_lca_toggle)
                alternar_estado_no(id_lca_toggle, lista_prod_lca_toggle, qtd_prod_lca_toggle) # Adiciona/considera o LCA

        respostas_finais[indice_original_consulta] = total_pares_coprimos_caminho_atual

        if lca_param != -1: # Remove/reverte o estado do LCA
             id_lca_toggle = lca_param
             if 0 <= id_lca_toggle < num_nos_param: # Checagem de limite
                lista_prod_lca_toggle = detalhes_produtos_fatores_por_no[id_lca_toggle]
                qtd_prod_lca_toggle = len(lista_prod_lca_toggle)
                alternar_estado_no(id_lca_toggle, lista_prod_lca_toggle, qtd_prod_lca_toggle) # Remove/reverte o LCA

    for resultado in respostas_finais:
        print(resultado)

# Bloco principal que e executado quando o script e rodado
if __name__ == '__main__':

    first_multiple_input = input().rstrip().split()
    n = int(first_multiple_input[0])
    q_count = int(first_multiple_input[1])

    node_values = list(map(int, input().rstrip().split()))

    edge_list = []
    # Uma arvore com N nos tem N-1 arestas. Loop executa N-1 vezes se N > 0.
    for _ in range(n - 1 if n > 0 else 0):
        edge_list.append(list(map(int, input().rstrip().split())))
    
    queries_list = []
    for _ in range(q_count):
        u_v_query_input = input().rstrip().split() # Le a linha "u v"
        u_query = int(u_v_query_input[0])
        v_query = int(u_v_query_input[1])
        queries_list.append((u_query, v_query))

    # Chama a funcao principal de resolucao 
    resolver(n, q_count, node_values, edge_list, queries_list)
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
