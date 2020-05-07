---
layout: default
title: Sobre mi
permalink: /archivo/
---
<div class="posts">
<h1>Archivo</h1>
<ul>
{% for post in site.posts %}
	<li>
		<a href="{{ post.url }}">{{ post.title }}</a>
		{% assign months = "Enero|Febrero|Marzo|Abril|Mayo|Junio|Julio|Agosto|Septiembre|Octubre|Noviembre|Diciembre" | split: "|" %}
        {% assign m = post.date | date: "%-m" | minus: 1 %}
        {% assign month = months[m] %}
        {% assign days = "Lunes|Martes|Miercoles|Jueves|Viernes|Sabado|Domingo" | split: "|" %}
        {% assign d = post.date | date: "%-a" | minus: 1 %}
        {% assign d_letter = days[a] %}
        {% assign day = post.date | date: "%d" %}
        {% assign year = post.date | date: "%Y" %}
        <span class="post_date">({{ day }} de {{ month }} del {{ year }})</span>
		<br>{{ post.description }}
	</li>
{% endfor %}
</ul>
</div>