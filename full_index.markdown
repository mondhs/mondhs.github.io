---
layout: default
title: Turinys, Index
---

<div class="col-md-6">
<h1>Turinys</h1>
{% for post in site.posts %}
{% if post.url contains '/lt/' %}
<ul class="list-inline">
	<li><a href="{{ post.url }}">[{{post.lang}}] {{ post.title | truncate:200 }}</a><p><small>{{ post.date | date: "%y-%m-%d" }}</small> <span class="entry">{{ post.summary }}</span></p></li>
</ul>	
{% endif %}
{% endfor %}
</div>
  <div class="col-md-6">
<h1>Index</h1>
{% for post in site.posts%}
{% if post.url contains '/en/' %}
<ul class="list-inline">
	<li><a href="{{ post.url }}">[{{post.lang}}] {{ post.title | truncate:200 }}</a><p><small>{{ post.date | date: "%y-%m-%d" }}</small> <span class="entry">{{ post.summary }}</span></p></li>
</ul>	
{% endif %}
{% endfor %}
</div>
