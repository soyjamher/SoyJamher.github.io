---
layout: default
title: Sobre mi
permalink: /blog/
---
<div class="posts">
<h1>Archivo</h1>
<ul>
{% for post in site.posts %}
	<li>
		<a href="{{ post.url }}">{{ post.title }}</a>({{ post.date | date_to_string }})
		<br>{{ post.description }}
	</li>
{% endfor %}
</ul>
</div>