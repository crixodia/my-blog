---
layout: post
title: "La mejor forma de contar números primos"
date: 2021-07-05 00:00:00 -0500
categories: algorithms python tutorial guide
language: es
author: Cristian Bastidas
image: /assets/images/thumbnail-primes.jpg
---
{%- assign classic = "/assets/images/posts/prime-classic.webp" -%}
{%- assign sieve = "/assets/images/posts/prime-sieve.webp" -%}
<script src="https://polyfill.io/v3/polyfill.min.js?features=es6"></script>
<script id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@3.0.1/es5/tex-mml-chtml.js"></script>

Incluso en los cursos básicos de programación, nos encontramos con este problema. Contar números primos puede ser una solución fácil si recurrimos a un enfoque clásico. Pero, ¿has pensado en el coste computacional o la rapidez del algoritmo? En esta ocasión te traigo dos enfoques para solucionar este problema y un análisis sobre cuál es la mejor solución.

Antes, hay que repasar el concepto de número primo brevemente. Los números primos con aquellos números enteros positivos que son divisibles únicamente para 1 y para sí mismos

Tomando en cuenta eso, contar los números primos hasta n tiene una fácil solución. Esta solución es el enfoque clásico comunmente usado cuando empezamos a programar.

Primero definimos una función que determine si el número es primo o no de la siguiente forma.

```python
def isPrime(n:int) -> bool:
  if n <= 1:
      return False
  for i in range(2,int(sqrt(n))+1):
    if n%i == 0:
      return False
  return True
```

Luego, definimos otra función que se encargue de contar los números primos preguntando desde 1 hasta $$n$$ usando la función anterior.

```python

def countPrimesClassic(n:int)->int:
  c = 0
  for i in range(2,n):
    if isPrime(i):
      c+=1
  return c

countPrimesClassic(500000)
```

Este método es el que probablemente hemos usado cuando empezábamos a programar. Sin embargo, imagina que queremos contar los números primos existentes hasta 50000. Esto tomaría mucho tiempo de cómputo. De hecho en el peor de los casos cuando el número es primo la primera función se comporta como $$O(\sqrt n)$$ asintóticamente. Y al ejecutarse dentro de un ciclo el cual es $$O(n)$$ el algoritmo implementado con las dos funciones tomaría $$O(n\sqrt n)$$ que si bien no es cuadrático, es lo suficientemente lento como para utilizarlo con valores de $$n$$ grandes.

Ahora, en muchas escuelas suelen enseñar la criba de Eratóstenes. Este método es el enfoque que usaremos para la siguiente alternativa. Repasemos en que consiste este método.

La criba de Eratóstenes nos permite encontrar números primos menores a un número dado. Básicamente se ajusta perfectamente al problema que queremos resolver. Su algoritmo prosigue de la siguiente forma:

1. Listar todos los números desde el 2 hasta el n deseado.
2. Tomar el primer número no marcado o descartado como primo.
3. Descartar todos los números múltiplos de aquel que se acaba de indicar como primo.
4. Si el cuadrado del primer número que no ha sido rayado ni marcado es inferior a n, entonces se repite el segundo paso. Si no, el algoritmo termina, y todos los enteros no tachados son declarados primos tal y como se observa en la siguiente animación.

<img src="https://upload.wikimedia.org/wikipedia/commons/b/b9/Sieve_of_Eratosthenes_animation.gif" alt="Prueba de escritorio" width="500px" style="display:block; margin-left: auto; margin-right:auto;">
<div style="font-size: x-small; text-align:center;">Criba de Eratóstenes desde <a href="https://upload.wikimedia.org/wikipedia/commons/b/b9/Sieve_of_Eratosthenes_animation.gif" title="Wikipedia">Wikipedia</a>
</div>
<br>

Una vez explicado el método, a continuación tienes el algoritmo.

```python
from math import sqrt
def countPrimesSieve(n: int) -> int:
    if n <= 2: return 0
    np, ans = [False]*n, 1
    for i in range(3, int(sqrt(n))+1, 2):
        if np[i]: continue
        for j in range(i*i, n, 2*i): np[j] = True
    for i in range(3, n, 2):
        if not np[i]: ans += 1
    return ans
  
countPrimesSieve(500000)
```

Para demostrar los resultados de forma contundente, ejecutamos una prueba de cada algoritmo desde 1 hasta 5000 con 5 muestras para cada n y obtengamos el tiempo requerido.

Para el algoritmo de enfoque clásico tienes la siguiente gráfica.

<img src="{{- classic | relative_url -}}" alt="Prueba de escritorio" width="500px" style="display:block; margin-left: auto; margin-right:auto;">
<div style="font-size: x-small; text-align:center;">Tiempo de ejecución para método clásico</div>
<br>


Para el algoritmo basado en la criba de Eratóstenes tenemos lo siguiente.

<img src="{{- sieve | relative_url -}}" alt="Prueba de escritorio" width="500px" style="display:block; margin-left: auto; margin-right:auto;">
<div style="font-size: x-small; text-align:center;">Tiempo de ejecución para criba de Eratóstenes</div>
<br>


A pesar de que ambos muestran una forma similar, es claramente visible quién toma menos tiempo  de ejecución. El segundo algoritmo toma en promedio 1 segundo menos en ejecutarse en comparación con el algoritmo 1 y como máximo 9 segundos menos, esto cuando llegamos hasta n = 5000.

Eso fue todo por esta entrega, y con ello te llevas un buen método para contar números primos. Quizá el problema no se requiera en el campo profesional. Sin embargo, puede ser útil para repasar conceptos y cambiar un poco los paradigmas al diseñar nuestros algoritmos.

Si te gustó o resultó de utilidad esta entrada, no olvides dejar tu comentario o dudas y seguirme en mis redes sociales para contenido similar.