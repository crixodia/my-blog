---
layout: post
title: "Python: Particiones de un número con método recursivo"
date: 2021-06-06 00:00:00 -0500
categories: algorithms python challenge guide
language: es
author: Cristian Bastidas
image: /assets/images/thumbnail-partitions.jpg
---
{%- assign partitions_sample = "/assets/images/posts/partitions.webp" -%}
{%- assign partitions_times = "/assets/images/posts/partitions_times.webp" -%}
{%- assign partitions_values = "/assets/images/posts/partitions_values.webp" -%}

<script src="https://polyfill.io/v3/polyfill.min.js?features=es6"></script>
<script id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@3.0.1/es5/tex-mml-chtml.js"></script>

- [Ejemplo](#ejemplo)
- [Código](#código)
- [Análisis de costo computacional](#análisis-de-costo-computacional)

Como aspirante a ingeniero, suelo resolver a diario retos de programación que me ayuden a mejorar mis habilidades. Tenía una buena racha hasta que me encontré con el siguiente problema.

> En teoría de números, una particion de un entero positivo `n` es una suma de enteros positivos. Dos sumas que solo difieren en el orden de los sumandos se consideran la misma partición.
> Crear una función que reciba un entero `x`. La función debe retornar el número de particiones que tiene `x`.

## Ejemplo

Para las particiones de `4` retornará 5. Pues las particiones de `4` son:

```python
[4] -> 4
[3, 1] -> 3 + 1 = 4
[2, 2] -> 2 + 2 = 4
[2, 1, 1] -> 2 + 1 + 1 = 4
[1, 1, 1, 1] -> 1 + 1 + 1 + 1 = 4
```

Para `8` tenemos 22 particiones que son:

```python
[8] -> 8
[7, 1] -> 7 + 1 = 8
[6, 2] -> 6 + 2 = 8
[6, 1, 1] -> 6 + 1 + 1 = 8
[5, 3] -> 5 + 3 = 8
[5, 2, 1] -> 5 + 2 + 1 = 8
[5, 1, 1, 1] -> 5 + 1 + 1 = 8
[4, 4] -> 4 + 4 = 8
[4, 3, 1] -> 4 + 3 + 1 = 8
[4, 2, 2] -> 4 + 2 + 2 = 8
[4, 2, 1, 1] -> 4 + 2 + 1 + 1 = 8
[4, 1, 1, 1, 1] -> 4 + 1 + 1 + 1 + 1 = 8
[3, 3, 2] -> 3 + 3 + 2 = 8
[3, 3, 1, 1] -> 3 + 3 + 1 + 1 = 8
[3, 2, 2, 1] -> 3 + 2 + 2 +1 = 8
[3, 2, 1, 1, 1] -> 3 + 2 + 1 + 1 + 1= 8
[3, 1, 1, 1, 1, 1] -> 3 + 1 + 1 + 1 + 1 + 1= 8
[2, 2, 2, 2] -> 2 + 2 + 2 + 2 = 8
[2, 2, 2, 1, 1] -> 2 + 2 + 2 + 1 + 1 = 8
[2, 2, 1, 1, 1, 1] -> 2 + 2 + 1 + 1 + 1 + 1 = 8
[2, 1, 1, 1, 1, 1, 1] -> 2 + 1 + 1 + 1 + 1 + 1 + 1 = 8
[1, 1, 1, 1, 1, 1, 1, 1] -> 1 + 1 + 1 + 1 + 1 + 1 + 1 + 1 = 8
```

Si bien el enfoque iterativo sería más efectivo cuando hablamos de coste computacional. Decidí apostar por el enfoque recursivo. ¿Por qué? Sucede que cuando observamos las partes de `5`, si empezamos por la particion formada de unos `[1,1,1,1,1]` y calculamos las posibles particiones que se pueden generar con esa cantidad de unos lo tendremos más sencillo. Esto es a lo que llamamos un enfoque "Bottom-up" dado a que partimos desde el que creemos es el último valor hacia el primero. Es importante recordar que si solo cambia el orden de los sumandos, se trata de una misma partición. Como `[3,2]` es lo mismo que `[2,3]` no es necesario que contemos dos veces esta partición. El siguiente gráfico es una especie de prueba de escritorio para este enfoque.

<img src="{{- partitions_sample | relative_url -}}" alt="Prueba de escritorio" width="500px" style="display:block; margin-left: auto; margin-right:auto;">
<div style="font-size: x-small; text-align:center;">Prueba de escritorio para particiones de 5</div>

## Código

Una vez introducida la metodología que aplicaremos para obtener las particiones, pasemos a codificar. Con Python resolveremos esto de forma sencilla.

Crearemos una función que toma como parámetros una lista de unos que se reducirán en cada iteración `ones`, el número a estudiar para la partición `x`. Cabe recalcar que este número cambiará en cada llamada recursiva. Y una lista en donde se almacenarán todas las particiones `origin`. La cual modificaremos por referencia.

```python
def partitions(ones:list, x:int, origin:list=[]) -> list:
  total = []
  for i in range(ones.count(1),1,-1):
    aux = ones[:ones.index(1)]
    aux.append(i)
    while sum(aux) < x:
      aux.append(1)
    if not sorted(aux) in origin:
      total.append(aux)
      origin.append(sorted(aux))
  for l in total:
    total = total + partitions(l,x,origin)
  return total
```

Luego creamos un método que ordene las particiones para obtener un resultado legible. Desde esta función arrancará la generadora de particiones `partitions`.

```python
def listPartitions(x:int) -> list:
  ones = [1]*x
  parts = partitions(ones,x,[])
  parts.append(ones)
  return parts
```

Para contar las particiones me decanté por simplemente contar los elementos de la lista de particiones. Esto no resulta óptimo pues sacrificaremos mucha memoria. Por lo cual es mejor el conteo de particiones sin usar listas con el mismo método recursivo aunque en vez de agregar las particiones acumularemos la cantidad total. Por el momento tenemos el método `countPartitions`.

```python
def countPartitions(x:int) -> int:
  if x < 0 or x >= 255:
    return -1
  else:
    return len(listPartitions(x))
```
## Análisis de costo computacional

Dado al uso de la recursividad como método, decidí analizar el tiempo de ejcución del algoritmo desde `n=0` hasta `n=44`. Puedes descargar el archivo [.csv](https://raw.githubusercontent.com/crixodia/partitions/main/partitions_ex_time.csv)

De este análisis obtenemos que el tiempo de ejcución crece de forma exponencial tal y como se muestra a continuación.

<img src="{{- partitions_times | relative_url -}}" alt="Tiempo Vs. N" width="500px" style="display:block; margin-left: auto; margin-right:auto;">
<div style="font-size: x-small; text-align:center;">Tiempo de jecución en milisegundos en función de n</div>
<br>

Con esto concluimos, que este algoritmo es ineficiente. Adicionalmente, analicemos la forma en que crece el número de partes. Esta también resulta exponencial.

<img src="{{- partitions_values | relative_url -}}" alt="Número de partes Vs. N" width="500px" style="display:block; margin-left: auto; margin-right:auto;">
<div style="font-size: x-small; text-align:center;">Número de partes en función de n</div>
<br>

Si te gustó este post no olvides dejar tu comentario o sugerencia. Además recuerda que puedes descargar el código y recursos en mi perfil de [GitHub](http://github.com/crixodia/partitions)