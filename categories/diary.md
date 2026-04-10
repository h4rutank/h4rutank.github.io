---
layout: page
title: "Diary"
permalink: /categories/diary/
---

# Diary

Diary posts

<ul>
  {% for post in site.categories.diary %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
      <span>{{ post.date | date: "%Y-%m-%d" }}</span>
    </li>
  {% endfor %}
</ul>
