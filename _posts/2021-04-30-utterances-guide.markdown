---
layout: post
title: "Utterances, un sistema de comentarios fácil de usar"
date: 2021-04-30 11:55:00 -0500
categories: tool web tutorial guide
language: es
author: Cristian Bastidas
image: /assets/images/thumbnail-utterances.jpg
---
{%- assign create = "/assets/images/posts/utterances-create.webp" -%}
{%- assign install = "/assets/images/posts/utterances-install.webp" -%}

Al iniciar este blog lo primero que tenía en mente es el sistema de comentarios. Así que me decanté por lo más popular. Disqus ofrece varias características interesantes. Sin embargo, si tu proyecto no es algo de tal importancia como para pagar una suscripción mensual tengo la alternativa perfecta para ti.

[Utterances](https://utteranc.es/) es un sistema de comentarios construido a partir de *GitHub issues* para blogs, y webs en general. Este sistema se destaca por ser completamente gratuito y ligero.

Usarlo es muy sencillo, basta seguir esta guía o las instrucciones de su [web oficial](https://utteranc.es/).

Crea un repositorio público en [GitHub](https://github.com) en donde alojar los comentarios. También puedes usar un repositorio existente.

<img src="{{- create | relative_url -}}" alt="Crear repositorio" width="600px" style="display:block; margin-left: auto; margin-right:auto;">

Instala la [aplicación](https://github.com/apps/utterances) de utterances en tu repositorio.

<img src="{{- install | relative_url -}}" alt="Install utterances" width="auto" style="display:block; margin-left: auto; margin-right:auto;">

Copia el siguiente script en la sección donde quieras los comentarios.

```html
<script src="https://utteranc.es/client.js"
        repo="[ENTER REPO HERE]"
        issue-term="pathname"
        theme="github-light"
        crossorigin="anonymous"
        async>
</script>
```

Usa tu propia información. En `repo` deberás usar tu nombre de usuario de GitHub y el nombre del repositorio de la siguiente forma `usuario/repositorio`. Por ejemplo:

```javascript
repo="crixodia/mi-blog"
```

Hazlo tuyo. Dispones de varios temas para la sección de comentarios.

```javascript
theme="github-dark"
```

Además, puedes modificar el formato de los *issues* en GitHub. Para ello usa el asistente de configuración de [utterances](https://utteranc.es/).

<img src="https://i.imgur.com/LJz21uP.gif" alt="Utterances config" width="auto" style="display:block; margin-left: auto; margin-right:auto;">

Eso es todo. Aunque hay que mencionar un par de desventajas. No es posible comentar de forma anónima y los usuarios deberán tener una cuenta en GitHub para comentar. Pero esto no algo de lo cual preocuparse si estás generando contenido o creando una comunidad en torno a desarrollo.

No olvides dejar tus inquietudes en los comentarios o escribirme a [@crixodia](htpps://twitter.com/crixodia) 🧐.

<div style="margin-left: auto; text-align:right;">
<i><small>
This article is also in <a href="{{ site.baseurl }}{% link en/_posts/2021-05-31-utterances-guide.markdown %}">English</a>
</small></i>
</div>