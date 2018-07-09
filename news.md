---
title: Noticias
layout: default
permalink: /news/
menu_order: 5
---

{% for post in site.posts %}
## [{{ post.title }}]({{ post.url | absolute_url }})
{{ post.excerpt }}

Publicado el {{ post.date | date_to_string }}.
{% endfor %}
