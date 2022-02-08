---
layout: page
title: Posts
permalink: /posts/
---
{% for post in site.posts %}
  {% assign currentdate = post.date | date: "%Y" %}
  {% if currentdate != date %}
      <h2> {{ currentdate }} </h2>
    {% assign date = currentdate %}
  {% endif %}
  <a href="{{ post.url }}">{{ post.title }}</a>
{% endfor %}
