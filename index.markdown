---
layout: default
title: Pradžios puslapis
lang: lt
categories:
- lt
---

<h2>Straipsniai</h2>
{% for post in site.posts limit:5 %}
<ul class="list-inline">
	<li><a href="{{ post.url }}">{{ post.title | truncate:200 }}</a><p><small>{{ post.date | date: "%y-%m-%d" }}</small> <span class="entry">{{ post.summary }}</span></p></li>
</ul>	
{% endfor %}
