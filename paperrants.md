---
layout: page
title: Paper Rants
permalink: /paperrants/
---

<p align="center">
  I like to read and explain research clearly. My interpretations of the various conundrums involved in AI research can be found in this section. 
</p>

<div class="home">

  <ul class="post-list">
    {% for paper in site.categories.papers %}
    <li>
        <div>
        <a class="post-link" href="{{ site.url }}{{ paper.url}}">{{ paper.title }}</a>
            <span class="post-date">{{ paper.date | date_to_long_string }}</span>
        </div>
    </li>
    {% endfor %}
  </ul>

</div>


