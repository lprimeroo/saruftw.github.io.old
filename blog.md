---
layout: page
title: Blog
permalink: /blog/
---

<div class="home">

  <ul class="post-list">
    {% for paper in site.categories.blogs %}
    <li>
        <div>
        <a class="post-link" href="{{ site.url }}{{ paper.url}}">{{ paper.title }}</a>
            <span class="post-date">{{ paper.date | date_to_long_string }}</span>
        </div>
    </li>
    {% endfor %}
  </ul>

</div>
