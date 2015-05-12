---
layout: home
title: "Welcome to Trail of Sparks!"
---
This site currently hosts [my blog](/posts) and some [music videos](music).

You may have wound up here because you tried to find my old website, **ryanbarringtoncox.com**. That site doesn't exist right now.

This blog is an experiment in [lateral thinking](/lateral-thinking), [writing publicly](/meta-blog/), [becoming healthier](/sleep-nutrition-exercise/), [not repeating myself](/keeping-it-dry/), [confronting fear](/public-speaking-and-living-with-fear/), [changing](/shifting-intent/) and [finding flow](/flow-breaker/).

--------

###Recent Posts
<ul>
  {% for post in site.posts limit: 5 %}  
    <li><a href="{{ site.url }}{{ post.url }}">{{ post.title }} {{ post.date | date: '%m/%d' }}</a></li>
  {% endfor %}
</ul>

{% include _view-all-posts.html %}
_______

###Some things from/about me hosted elsewhere on the internet

- [My audio albums](https://ryanbarringtoncox.bandcamp.com/) (music I recorded at home with lots of instruments)
- [More youtube videos of my music](https://www.youtube.com/user/ryanbarrybarrry)
- [A review of my last album, *Pool*](http://allimarshall.tumblr.com/post/73630833953/playing-pool-with-ryan-barrington-cox)
- [An article about my humorous speech contest experience, Fall of 2014](https://mountainx.com/blogwire/asheville-toastmasters-club-436-members-take-prizes-in-nc-district-competition/)
- [A command line lyrics-grabber](https://github.com/ryanbarringtoncox/command_line_lyrics)
- [A website I built for my Ice Cream Shop Friends](http://thehopicecreamcafe.com/)
- [A command line program that shuffles up lyrics into a collage](https://github.com/ryanbarringtoncox/lyric-shuffler)
