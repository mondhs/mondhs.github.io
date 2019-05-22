---
layout: default
title: Mano Puslapis, My Page
lang: lt
categories:
- lt
---

<div class="jumbotron">
  <h1 class="text-center"> {'lt':'Sveiki'}, {'en':'Hello'} </h1>
    <p class="text-center">The Answer to the Ultimate Question of Life, the Universe, and Everything</p>
    <p class="text-center"><a href="http://en.wikipedia.org/wiki/42_%28number%29"><span class="badge badge-secondary">42</span></a></p>
</div>

<div class="row">
  <div class="col">
  {% for post in site.posts limit:5 %}
  {% if post.url contains '/lt/' %}
  <ul class="list-inline">
  	<li><a href="{{ post.url }}">[{{post.lang}}] {{ post.title | truncate:200 }}</a><p><small>{{ post.date | date: "%y-%m-%d" }}</small> <span class="entry">{{ post.summary }}</span></p></li>
  </ul>
  {% endif %}
  {% endfor %}
  </div>
    <div class="col">
  {% for post in site.posts limit:5 %}
  {% if post.url contains '/en/' %}
  <ul class="list-inline">
  	<li><a href="{{ post.url }}">[{{post.lang}}] {{ post.title | truncate:200 }}</a><p><small>{{ post.date | date: "%y-%m-%d" }}</small> <span class="entry">{{ post.summary }}</span></p></li>
  </ul>
  {% endif %}
  {% endfor %}
  </div>
</div>
<div class="row">
  <div class="col">
    <p class="text-center"><a href="/full_index/"><span class="iconify" data-icon="octicon:chevron-down" data-inline="false"></span></a></p>
  </div>
</div>
