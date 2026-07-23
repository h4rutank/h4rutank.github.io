---
layout: page
title: "Categories"
permalink: /categories/
---

父方が東京なので、上京とか都会への憧れを抱いてる人に優越感を感じてました（８歳）<br>
「上京したことないから、東京の歌は書けない。」得意げに笑うアタシ...って感じのガキのまま大人になりました。<br>

<ul>
  {% for category in site.categories %}
    <li>
      <a href="/categories/{{ category[0] }}/">
        {{ category[0] }} ({{ category[1].size }})
      </a>
    </li>
  {% endfor %}
</ul>
