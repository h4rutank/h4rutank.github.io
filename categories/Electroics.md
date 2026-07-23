---
layout: default
title: "Electronics"
permalink: /categories/Electronics/
---

**電子工作**<br>

コンプレックスで続けて、仕事になりました。<br>

<ul>
  {% for post in site.categories.Electronics %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
      <span>{{ post.date | date: "%Y-%m-%d" }}</span>
    </li>
  {% endfor %}
</ul>
