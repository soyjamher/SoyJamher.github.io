---
layout: default
title: Sobre mi
permalink: /blog/
---
<div class="posts">
  {% for post in site.posts %}
    <article class="post">
    	<ul class="post-list">
    		<li>{{ post.title }}<small>{{ post.date | date: "%B %e, %Y" }}</small></li>
    	</ul>
    </article>
  {% endfor %}
</div>