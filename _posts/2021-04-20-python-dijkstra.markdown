---
layout: post
title: "Algoritmo de Dijkstra en Python"
date: 2021-04-20 11:55:00 -0500
categories: algorithm python tutorial wiki
language: es
image: /assets/images/thumbnail-dijkstra.webp
---
{%- assign graph = "/assets/images/posts/wgraph.webp" -%}
{%- assign dijkstra = "/assets/images/posts/dijkstra-animation.webp" -%}
<script src="https://polyfill.io/v3/polyfill.min.js?features=es6"></script>
<script id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@3.0.1/es5/tex-mml-chtml.js"></script>

Si estás estudiando ingeniería o ciencias de la computación, lo más probable es que te hayas cruzado con este tema en teoría de grafos. Pues este tiene una amplia colección de usos en los campos mencionados.

- [Algoritmo](#algoritmo)
- [Código](#código)
- [Módulo en Python](#módulo-en-python)
  - [Encontrar todas las distancias y caminos](#encontrar-todas-las-distancias-y-caminos)
  - [Encontrar los caminos más cortos](#encontrar-los-caminos-más-cortos)
  - [Encontrar las distancias más cortas](#encontrar-las-distancias-más-cortas)

El algoritmo de Dijkstra tiene como objetivo encontrar el camino más corto entre nodos en un grafo. Este grafo puede representar una red o el costo de viaje entre ciudades usando determinados caminos. Este algoritmo usa las etiquetas con número positivos (enteros o reales) que representan el costo en cada arista.

La idea radica en explorar todos los caminos más cortos desde el vértice origen hasta los vértices restantes. Cuando se obtiene el camino más corto hacia el vértice deseado o todos los caminos cortos a los demás vértices que componen el grafo, este se detiene. Es de importancia mencionar que este algoritmo no funciona para costes negativos. Para este fin existe el algoritmo de Bellman-Ford.

## Algoritmo

Sea un grafo ponderado de $$ N $$ nodos no aislados, $$X$$ el nodo inicial. Un vector $$V$$ de dimensión $$N$$ hará la función de almacén de las distancias de $$X$$ hacia los nodos restantes. Luego, el algoritmo se plantea de la siguiente forma:


<img src="{{- dijkstra | relative_url -}}" alt="Algoritmo de Dijkstra animado" width="250px" style="display:block; margin-left: auto; margin-right:auto;">
<div style="font-size: x-small; text-align:center;">Algortimo de Dijkstra animado extraído de <a href="https://en.wikipedia.org/wiki/Dijkstra%27s_algorithm#/media/File:Dijkstra_Animation.gif" title="Wikipedia">Wikipedia</a>
</div>
<br>

1. Inicializar las distancias en $$V$$ con un valor infinito a excepción de $$X$$.
2. Se toma el nodo actual  $$A \leftarrow X$$.
3. Se recorren todos los nodos adyacentes de $$A$$, a excepción de los nodos visitados. Es decir, a los nodos no visitados $$\left \{ P_{1}, P_{2}, P_{3}, ..., P_{N} \right \}$$.
4. Luego, se calculará la posible distancia de A hacia sus vecinos con $$\mathrm{dp(} P_{i} \mathrm{)} = V_{A} + \mathrm{d(}A,V_{i}\mathrm{)}$$ Se puede interpretar como la distancia actual del nodo en $$V$$ más la distancia desde el nodo $$A$$ hasta el nodo $$P_{i}$$ Si esta distancia es menor a la distancia almacenada en $$V$$, el vector $$V$$ se actualiza con esta.
5. Se marca el nodo $$A$$ como ya visitado.
6. Se toma el próximo nodo actual $$A$$ como el menor en $$V$$ y se repite el proceso desde el paso 3. Esto mientras existan nodos no visitados.

## Código

El algoritmo en Python tomará una matriz de adyacencia, el nodo de origen y opcionalmente el nodo destino. Dado a que mencionamos la matriz de adyacencia. Es importante que tengas claro la construcción de esta a partir de un grafo ponderado.

Siguiendo los pasos del algoritmo mostrado, es de saber que es necesario crear métodos auxiliares durante la codificación. Lo que resulta en lo siguiente:

```python
def find_all(wmat, start, end=-1):
    n = len(wmat)

    dist = [inf]*n
    dist[start] = wmat[start][start]  # 0

    spVertex = [False]*n
    parent = [-1]*n

    path = [{}]*n

    for count in range(n-1):
        minix = inf
        u = 0

        for v in range(len(spVertex)):
            if spVertex[v] == False and dist[v] <= minix:
                minix = dist[v]
                u = v

        spVertex[u] = True
        for v in range(n):
            if not(spVertex[v]) and wmat[u][v] != 0 and dist[u] + wmat[u][v] < dist[v]:
                parent[v] = u
                dist[v] = dist[u] + wmat[u][v]

    for i in range(n):
        j = i
        s = []
        while parent[j] != -1:
            s.append(j)
            j = parent[j]
        s.append(start)
        path[i] = s[::-1]

    return (dist[end], path[end]) if end >= 0 else (dist, path)
```

Para comprobar su correcta ejecución tomemos el siguiente grafo:

<img src="{{- graph | relative_url -}}" alt="Grafo ponderado" style="display:block; margin-left: auto; margin-right:auto;">

Cuya matriz de adyacencia es:
<p>
$$W=\begin{bmatrix}
 0 &2  &0  &0  &0  &1  &0  &0 \\ 
 2 &0  &2  &2  &4  &0  &0  &0 \\ 
 0 &2  &0  &0  &3  &0  &0  &1 \\ 
 0 &2  &0  &0  &4  &3  &0  &0 \\ 
 0 &4  &3  &4  &0  &0  &7  &0 \\ 
 1 &0  &0  &3  &0  &0  &5  &0 \\ 
 0 &0  &0  &0  &7  &5  &0  &6 \\ 
 0 &0  &1  &0  &0  &0  &6  &0 
\end{bmatrix}$$
</p>

O en Python:

```python
wmat = [[0, 2, 0, 0, 0, 1, 0, 0],
        [2, 0, 2, 2, 4, 0, 0, 0],
        [0, 2, 0, 0, 3, 0, 0, 1],
        [0, 2, 0, 0, 4, 3, 0, 0],
        [0, 4, 3, 4, 0, 0, 7, 0],
        [1, 0, 0, 3, 0, 0, 5, 0],
        [0, 0, 0, 0, 7, 5, 0, 6],
        [0, 0, 1, 0, 0, 0, 6, 0]]
```

## Módulo en Python

Para una mejor comprensión puedes descargar el [módulo](http://github.com/crixodia/python-dijkstra) que contiene varios métodos en función del vértice destino y con las opciones agregadas para obtener solo una lista de distancias o una lista con los caminos. Para ello importaremos el módulo en nuestro directorio de trabajo.

```python
import dijkstra
```

### Encontrar todas las distancias y caminos

Tomemos como vértice de origen a `a` o en base a los índices de la matriz de adyacencia `0`

```python
dijkstra.find_all(wmat, 0)
```

Obteniendo una tupla con una lista de todas las distancias y otra con todos los caminos correspondientes a esas distancias de la forma `(distancias, caminos)`. Por ejemplo, `distancias[x]` es la distancias más corta hacia el vértice `x` cuyo camino es `caminos[x]`
```python
(
    # Distancias
    [0, 2, 4, 4, 6, 1, 6, 5],
    # Caminos
    [
        [0],
        [0, 1],
        [0, 1, 2],
        [0, 5, 3],
        [0, 1, 4],
        [0, 5],
        [0, 5, 6],
        [0, 1, 2, 7]
    ]
)
```
### Encontrar los caminos más cortos

```python
dijkstra.find_shortest_path(wmat, 0)
```

Si no especificamos el vértice destino, obtendremos todas las distancias hacia los vértices restantes.

```python
# Caminos
[
    [0],
    [0, 1],
    [0, 1, 2],
    [0, 5, 3],
    [0, 1, 4],
    [0, 5],
    [0, 5, 6],
    [0, 1, 2, 7]
]
```

Por otra parte, al especificar el vértice destino (en este caso 7)

```python
dijkstra.find_shortest_path(wmat, 0, 7)
```

Obtenemos:

```python
[0, 1, 2, 7]
```

### Encontrar las distancias más cortas

De forma análoga a los caminos más cortos tenemos todas las distancias si no especificamos el vértice destino.

```python
dijkstra.find_shortest_distance(wmat, 0)
```
Cuya salida es:

```python
[0, 2, 4, 4, 6, 1, 6, 5]
```

Si especificamos el vértice destino (7) obtendremos solo esa distancia.

```python
dijkstra.find_shortest_distance(wmat, 0, 7)
```

Es decir:

```python
5
```

Recuerda que puedes [descargar](https://github.com/crixodia/python-dijkstra) el módulo junto a sus respectiva documentación 🧐.
No olvides dejar tu comentario o escribirme a [@crixodia](https://twitter.com/crixodia) para sugerencias o dudas.