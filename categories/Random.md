---
layout: default
title: "Random"
permalink: /categories/Random/
---

**雑感**<br>

 <p>どこにだってあるものでも、こことそこじゃ違うので、ここにないからどこかにあると思って来ただけです。</p>
こそあど言葉を多用すると混乱して伝わらないから気を付けましょうって、小１で言われるじゃん。大人になったら忘れてたよ。


<ul>
  {% for post in site.categories.Random %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
      <span>{{ post.date | date: "%Y-%m-%d" }}</span>
    </li>
  {% endfor %}
</ul>
