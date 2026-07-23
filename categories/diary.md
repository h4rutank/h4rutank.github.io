---
layout: default
title: "Diary"
permalink: /categories/Diary/
---

**日記**<br>

ヘルシンキ生活の練習を買ったけど、帰りの電車で10ページくらい読んで、積読してる。<br>
いや、売りに出したかもしれない。<br>

他人の物語に対して、本人が感じている以上の価値を、感じ取ることは出来ない。<br>
もし、大きな意味を見出してしまったら、それは偶像です。推しには理念を宛がうからな。<br>

つまり、他人の生活なんて誰も興味ない。<br>

<ul>
  {% for post in site.categories.Diary %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
      <span>{{ post.date | date: "%Y-%m-%d" }}</span>
    </li>
  {% endfor %}
</ul>
