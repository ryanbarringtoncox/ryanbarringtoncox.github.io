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

- [My audio albums](https://ryanbarringtoncox.bandcamp.com/) (music I arranged and recorded)
- [My Youtube videos](https://www.youtube.com/watch?v=C_J1G3M-_jI&list=PLEP0Foq1SruN9ZA-dz9VbSYaLCF1gWnVP)
- [A review of my last album, *Pool*](http://allimarshall.tumblr.com/post/73630833953/playing-pool-with-ryan-barrington-cox)
- [An article about my humorous speech contest experience, Fall of 2014](https://mountainx.com/blogwire/asheville-toastmasters-club-436-members-take-prizes-in-nc-district-competition/)
- [A command line lyrics-grabber](https://github.com/ryanbarringtoncox/command_line_lyrics)
- [A command line program that shuffles up lyrics into a collage](https://github.com/ryanbarringtoncox/lyric-shuffler)
- [My band](https://ifyouwannas.bandcamp.com/)
