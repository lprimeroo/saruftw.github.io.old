---
layout: page
title: Blog
permalink: /blog/
---

<p align="center">
  I like to read and explain research clearly. My interpretations of the various conundrums involved in AI research can be found in this section.
</p>

<div class="home">

  <ul class="post-list">
    {% for post in site.personals %}
    <li>
        <div>
        <a class="post-link" href="{{ site.url }}{{ post.url}}">{{ post.title }}</a>
            <span class="post-date">{{ post.date | date_to_long_string }}</span>
        </div>
    </li>
    {% endfor %}
  </ul>

</div>
