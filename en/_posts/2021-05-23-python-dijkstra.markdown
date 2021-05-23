---
layout: post
title: "Dijkstra's algorithm in Python"
date: 2021-05-23 11:55:00 -0500
categories: algorithms python tutorial wiki
language: en
author: Cristian Bastidas
image: /assets/images/thumbnail-dijkstra.webp
---
{%- assign graph = "/assets/images/posts/wgraph.webp" -%}
{%- assign dijkstra = "/assets/images/posts/dijkstra-animation.webp" -%}
<script src="https://polyfill.io/v3/polyfill.min.js?features=es6"></script>
<script id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@3.0.1/es5/tex-mml-chtml.js"></script>

If you are studying engineering or computer science, it is probably that you have found this topic related to graph theory. It has a wide collection of uses in those fields.

- [Algorithm](#algorithm)
- [Code](#code)
- [Python module](#python-module)
- [Find all distances and paths](#find-all-distances-and-paths)
  - [Example code](#example-code)
- [Find the shortest path](#find-the-shortest-path)
  - [Example code with final vertex](#example-code-with-final-vertex)
  - [Example code without final vertex](#example-code-without-final-vertex)
- [Find the shortest distance](#find-the-shortest-distance)
  - [Example code with final node](#example-code-with-final-node)
  - [Example code without final node](#example-code-without-final-node)

The purpose of Dijkstra's algorithm is to find the shortest path between nodes on a graph. This graph can represent a network or the travel cost between cities using determined paths. This algorithm uses tags with positive numbers, which represent the cost of an edge.

The idea lies in exploring all the shortest paths from the origin node to the rest of the nodes. It halts when it gets the shortest path to the rest of the nodes or the target. It is important to remind that this algorithm does not work with negative costs. For this purpose, we have to implement the Bellman-Ford algorithm.

## Algorithm

Given a weighted graph of $$ N $$ nodes not isolated and the initial node $$X$$. An $$N$$ dimension vector $$V$$ will store the distances from $$X$$ to the rest of the nodes. Then, the algorithm will be:

<img src="{{- dijkstra | relative_url -}}" alt="Algoritmo de Dijkstra animado" width="250px" style="display:block; margin-left: auto; margin-right:auto;">
<div style="font-size: x-small; text-align:center;">From <a href="https://en.wikipedia.org/wiki/Dijkstra%27s_algorithm#/media/File:Dijkstra_Animation.gif" title="Wikipedia">Wikipedia</a>
</div>
<br>

1. Initialize distances on $$V$$ with the maximum value except for $$X$$.
2. The current node is $$A \leftarrow X$$.
3. Go over all adjacent nodes of $$A$$, except for visited nodes. That is unvisited nodes $$\left \{ P_{1}, P_{2}, P_{3}, ..., P_{N} \right \}$$.
4. Then, compute the possible distance from $$A$$ to its neighboors with $$\mathrm{dp(} P_{i} \mathrm{)} = V_{A} + \mathrm{d(}A,V_{i}\mathrm{)}$$ We could also interpret this as the current distance in $$V$$ plus the distance from $$A$$ to $$P_{i}$$ If this distance is shorter than the stored on $$V$$, the vector $$V$$ will be updated with this.
5. Check $$A$$ as visited.
6. The next current node $$A$$ will be the smaller on $$V$$ and repeat from 3 while there are not visited nodes.

## Code

The python algorithm will take an adjacency matrix, the initial node, and optionally the target node. Since we mentioned the adjacency matrix. It is important that you know how to create one from a weighted graph.

Following the algorithm before, it is required to create auxiliary methods during the coding phase, which results in:

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

In order to know if the algorithm is working fine, we will create an example of an adjacency matrix.

<img src="{{- graph | relative_url -}}" alt="Weighted graph" style="display:block; margin-left: auto; margin-right:auto;">

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

In python as:

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

## Python module

Download [the module](http://github.com/crixodia/python-dijkstra) and import it.

```python
import dijkstra
```

## Find all distances and paths

```python
dijkstra.find_all(wmat, start, end=-1):
```

Returns a tuple with a distances list and paths list of all remaining nodes with the same indexing.

        (distances, paths)

For example, distances[x] are the shortest distances from x node which shortest path is paths[x]. x is an element of {0, 1, ..., n-1} where n is the number of nodes

    Args:
    wmat    --  weighted graph's adjacency matrix
    start   --  paths' first node
    end     --  (optional) paths' end node. Return just the 
                distance and its path

    Exceptions:
    Index out of range, Be careful with "start" and "end" nodes.

### Example code

```python
print(dijkstra.find_all(wmat, 0))
```

Output:

```
([0, 2, 4, 4, 6, 1, 6, 5], [[0], [0, 1], [0, 1, 2], [0, 5, 3], [0, 1, 4], [0, 5], [0, 5, 6], [0, 1, 2, 7]])
```

## Find the shortest path

```python
dijkstra.find_shortest_path(wmat, start, end=-1):
```
Returns paths' list of all remaining nodes.

    Args:
    wmat    --  weighted graph's adjacency matrix
    start   --  paths' first node
    end     --  (optional) paths' end node. Returns just
                the path

    Exceptions:
    Index out of range, Be careful with "start" and "end" nodes.

### Example code with final vertex

```python
dijkstra.find_shortest_path(wmat, 0, 7)
```

Output:

```
[0, 1, 2, 7]
```

### Example code without final vertex

```python
dijkstra.find_shortest_path(wmat, 0)
```

Output:

```
[[0], [0, 1], [0, 1, 2], [0, 5, 3], [0, 1, 4], [0, 5], [0, 5, 6], [0, 1, 2, 7]]
```

## Find the shortest distance

```python
dijkstra.find_shortest_distance(wmat, start, end=-1):
```

Returns distances' list of all remaining nodes.

    Args:
    wmat    --  weighted graph's adjacency matrix
    start   --  paths' first node
    end     --  (optional) path's end node. Returns just
                the distance

    Exceptions:
    Index out of range, Be careful with "start" and "end" nodes.

### Example code with final node

```python
dijkstra.find_shortest_distance(wmat, 0, 7)
```

Output:

```
5
```

### Example code without final node

```python
dijkstra.find_shortest_distance(wmat, 0)
```

Output:
```
[0, 2, 4, 4, 6, 1, 6, 5]
```

[Download](https://github.com/crixodia/python-dijkstra) the module 🧐.
Please leave your comment and suggestions.

<div style="margin-left: auto; text-align:right;">
<i><small>
Este artículo también está en <a href="{{ site.baseurl }}{% link _posts/2021-04-20-python-dijkstra.markdown %}">español</a>
</small></i>
</div>