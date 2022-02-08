---
layout: page
title: Posts
permalink: /posts/
---
{% for post in site.posts %}
  {% assign currentdate = post.date | date: "%Y" %}
  {% if currentdate != date %}
    {% unless forloop.first %}</ul>{% endunless %}
    {{ currentdate }}
    {% assign date = currentdate %}
  {% endif %}
    <li><a href="{{ post.url }}">{{ post.title }}</a></li>
  {% if forloop.last %}{% endif %}
{% endfor %}
