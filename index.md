---
layout: home
title: "Welcome to Trail of Sparks!"
---

###Recent Blog Posts
<ul>
  {% for post in site.posts limit: 5 %}  
    <li><a href="{{ site.url }}{{ post.url }}">{{ post.title }} {{ post.date | date: '%m/%d/%Y' }}</a></li>
  {% endfor %}
</ul>

{% include _view-all-posts.html %}
_______

###Other things from/about me 

- [Audio albums](https://ryanbarringtoncox.bandcamp.com/)
- [Music videos](https://www.youtube.com/watch?v=C_J1G3M-_jI&list=PLEP0Foq1SruN9ZA-dz9VbSYaLCF1gWnVP)
- [A review of my last album, *Pool*](http://allimarshall.tumblr.com/post/73630833953/playing-pool-with-ryan-barrington-cox)
- [An article about my speech contest experience, Fall of 2014](https://mountainx.com/blogwire/asheville-toastmasters-club-436-members-take-prizes-in-nc-district-competition/)
- [My band](https://ifyouwannas.bandcamp.com/)
- [A command line lyrics-grabber](https://github.com/ryanbarringtoncox/command_line_lyrics)
