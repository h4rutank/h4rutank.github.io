---
layout: default
title: "Random"
permalink: /categories/Random/
---

雑記

<ul>
  {% for post in site.categories.Random %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
      <span>{{ post.date | date: "%Y-%m-%d" }}</span>
    </li>
  {% endfor %}
</ul>
