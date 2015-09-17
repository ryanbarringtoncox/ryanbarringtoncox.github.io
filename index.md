---
layout: home
title: "Welcome to Trail of Sparks!"
---

###Recent Blog Posts
<ul>
  {% for post in site.posts limit: 10 %}  
    <li><a href="{{ site.url }}{{ post.url }}">{{ post.title }} {{ post.date | date: '%m/%d/%Y' }}</a></li>
  {% endfor %}
</ul>

{% include _view-all-posts.html %}
