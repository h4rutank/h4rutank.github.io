---
layout: default
title: "Diary"
permalink: /categories/Diary/
---

日記

<ul>
  {% for post in site.categories.diary %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
      <span>{{ post.date | date: "%Y-%m-%d" }}</span>
    </li>
  {% endfor %}
</ul>
