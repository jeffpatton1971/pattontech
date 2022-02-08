---
layout: page
title: Posts
permalink: /posts/
---
{% for post in site.posts %}
<ul>
  {% assign currentdate = post.date | date: "%Y" %}
  {% if currentdate != date %}
      {{ currentdate }}
    {% assign date = currentdate %}
  {% endif %}
  <li><a href="{{ post.url }}">{{ post.title }}</a></li>
{% endfor %}
</ul>
