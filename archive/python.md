---
layout: page
title: Python
permalink: /archive/python.html
---

[홈]({{ '/' | relative_url }}) › [Archive]({{ '/archive.html' | relative_url }}) › Python

{% for post in site.categories.python %}
- {{ post.date | date: "%Y-%m-%d" }} · [{{ post.title }}]({{ post.url | relative_url }})
{% endfor %}
