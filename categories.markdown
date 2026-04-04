---
layout: page
title: "Categories"
permalink: /categories/
---

# Categories

Here's a catefory list of posts.

<ul>
  {% for category in site.categories %}
    <li>
      <a href="/categories/{{ category[0] }}/">
        {{ category[0] }} ({{ category[1].size }})
      </a>
    </li>
  {% endfor %}
</ul>
