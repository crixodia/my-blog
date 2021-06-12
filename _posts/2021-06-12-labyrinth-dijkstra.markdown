---
layout: post
title: "Resolviendo laberintos con dijkstra en Python"
date: 2021-06-12 00:00:00 -0500
categories: algorithms python challenge guide
language: es
author: Cristian Bastidas
image: /assets/images/thumbnail-labyrinth.webp
---

```python
def mapNodes(x:list, init:chr='a') -> list:
  m, cords = [], []
  label = ord(init)
  for i in range(len(x)):
    m.append([])
    for j in range(len(x[i])):
      if x[i][j]:
        m[i].append(chr(label))
        cords.append((i,j))
        label += 1
      else:
        m[i].append(None)
  return m, cords
```

```python
def transpose(matrix:list):
  t_matrix = [[0]*len(matrix) for i in range(len(matrix[0]))]
  for i in range(len(matrix)):
    for j in range(len(matrix[0])):
      t_matrix[j][i] = matrix[i][j]
  return t_matrix
```

```python
def rotate(matrix:list, comparator='-'):
  return transpose(matrix[::-1])
```

```python
def getEdges(x:list, labels:list=[], rotated:bool=False) -> list:
  edges = [] 
  for i in range(len(x)):
    for j in range(len(x[i])):
      if x[i][j]:
        if i+1 < len(x) and j+1 < len(x[i]):
          if x[i][j+1]:
            e = sorted([labels[i][j], labels[i][j+1]])
            if not e in edges:
              edges.append(e)
          if x[i+1][j]:
            e = sorted([labels[i][j], labels[i+1][j]])
            if not e in edges:
              edges.append(e)
          if x[i+1][j+1]:
            e = sorted([labels[i][j], labels[i+1][j+1]])
            if not e in edges:
              edges.append(e)
        elif i+1 < len(x):
          if x[i+1][j]:
            e = sorted([labels[i][j], labels[i+1][j]])
            if not e in edges:
              edges.append(e)
        elif j+1 < len(x[i]):
          if x[i][j+1]:
            e = sorted([labels[i][j], labels[i][j+1]])
            if not e in edges:
              edges.append(e)

  if not rotated:
    edges = edges + getEdges(rotate(x), rotate(labels), True)

  return edges
```

```python
def edge2node(edges:list) -> list:
  nodes = []
  for e in edges:
    for node in e:
      if not node in nodes:
        nodes.append(node)
  return nodes
```

```python
def matrix2line(matrix:list) -> list:
  line = []
  for row in matrix:
      for col in row:
        if col != None:
          line.append(col)
  return line
```

```python
def createWGraph(x:list) -> list:
  labels, coords = mapNodes(x)

  edges = getEdges(x, labels)
  nodes = edge2node(edges)

  n = len(nodes)
  w = [[0]*n for i in range(n)]

  for e in edges:
    i, j = nodes.index(e[0]), nodes.index(e[1])
    w[i][j], w[j][i] = 1, 1

  return w, nodes, edges, coords, matrix2line(labels)
```

```python
def getMinPath(x:list, format:str='label') -> int:
  w, nodes, edges, coords, labels = createWGraph(x)
  last = nodes.index(max(nodes))

  d, path = dijkstra(w, 0, last)

  if format != 'd_index':
    if format == 'label':
      path = [nodes[i] for i in path]
    elif format == 'coords':
      path = [coords[labels.index(nodes[i])] for i in path]
  
  return d, path
```

```python
a = [
        [True, False, False, False, True],
        [False, True, True, True, False],
        [False, False, True, False, True],
        [True, False, True, True, True]
    ]
print(getMinPath(a, format='coords'))
```

```python
a = [
        [True, True, True, True, True, False],
        [False, False, False, True, True, False],
        [False, True, True, False, False, True],
        [True, False, False, False, False, False],
        [True, True, True, True, True, True]
    ]

print(getMinPath(a,'coords'))
```