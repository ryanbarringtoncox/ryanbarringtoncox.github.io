---
layout: home
title: "Welcome to Trail of Sparks!"
---

### Blog Posts

<ul>
  {% for post in site.posts %}  
    <li><a href="{{ site.url }}{{ post.url }}">{{ post.title }} - {{ post.date | date: '%m/%d/%Y' }}</a></li>
  {% endfor %}
</ul>
