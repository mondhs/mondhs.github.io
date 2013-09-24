---
layout: default
title: Pradžios puslapis
lang: lt
categories:
- lt
---


{% for post in site.posts limit:5 %}
<dl class="dl-horizontal">
	<dt><a href="{{ post.url }}">{{ post.title | truncate:200 }}</a></dt>
	<dd><small>{{ post.date | date: "%y-%m-%d" }}</small> <span class="entry">{{ post.summary }}</span></dd>
</dl>	
{% endfor %}
