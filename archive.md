---
layout: page
title: Archive
permalink: /archive.html
---

## 카테고리

{% assign cats = site.categories | sort %}
{% for c in cats %}
- [{{ c[0] }} ({{ c[1].size }})]({{ '/archive/' | append: c[0] | append: '.html' | relative_url }})
{% endfor %}

## 전체 글

{% for post in site.posts %}
- {{ post.date | date: "%Y-%m-%d" }} · [{{ post.title }}]({{ post.url | relative_url }})
{% endfor %}
