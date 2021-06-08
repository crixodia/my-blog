---
layout: post
title: "Utterances, the easiest comment system"
date: 2021-05-31 11:55:00 -0500
categories: tool web tutorial guide
language: en
author: Cristian Bastidas
image: /assets/images/default-featured-image.png
---
{%- assign create = "/assets/images/posts/utterances-create.webp" -%}
{%- assign install = "/assets/images/posts/utterances-install.webp" -%}

When I started this blog, the first thing I thought of was the comment system. At first, I went with Disqus. Unfortunately, you have to pay for this if you want a clean blog (without ads). That is why I will show you an awesome alternative.

Utterances is a comments system built from *GitHub Issues* for blogs and other webs. This is completely free and lightweight. It is easy to use and, it is enough if you follow the instructions on the official [web page](https://utteranc.es/).

You have to create a public repository on [GitHub](https://github.com). You could also use an old one.

<img src="{{- create | relative_url -}}" alt="Create a repository" width="600px" style="display:block; margin-left: auto; margin-right:auto;">

Install [utterances app](https://github.com/apps/utterances) in your repository.

<img src="{{- install | relative_url -}}" alt="Install utterances" width="auto" style="display:block; margin-left: auto; margin-right:auto;">

Copy this script on your web page comment section.

```html
<script src="https://utteranc.es/client.js"
        repo="[ENTER REPO HERE]"
        issue-term="pathname"
        theme="github-light"
        crossorigin="anonymous"
        async>
</script>
```

You must use your own information. You have to provide your repo name on `repo` with your username following this format `user/repo_name`.

```javascript
repo="crixodia/mi-blog"
```
Make it your own. You could customize it through the `theme` field.

```javascript
theme="github-dark"
```

Furthermore, you can modify *issues* format on GitHub. For this purpose use the [wizard](https://utteranc.es/).

<img src="https://i.imgur.com/LJz21uP.gif" alt="Utterances config" width="auto" style="display:block; margin-left: auto; margin-right:auto;">

All done. Although, we have to notice one disadvantage. It is not possible to leave a comment without a GitHub account. But it does not matter if you are creating content for developers.

Please, leave your comment (test it 😉), and do not forget to follow me on [@twitter](htpps://twitter.com/crixodia) 🧐.

<div style="margin-left: auto; text-align:right;">
<i><small>
Este artículo también está en <a href="{{ site.baseurl }}{% link _posts/2021-04-30-utterances-guide.markdown %}">español</a>
</small></i>
</div>