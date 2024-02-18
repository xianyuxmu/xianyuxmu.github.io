---
title:
layout: page
---

<h1 class="main-color" style="font-size: 1em; font-weight: 400;">
这是陈烨彬 Robin Chen 的个人主页！
</h1>

本博客用于记录和分享我见过的人、读过的书及我个人的思考。

<blockquote class="blockquote-center">
<p style="font-size: 1.3em; font-weight: 600;">Be as you wish to seem.</p>
<p style="font-style: italic;"><strong> —— Socrates</strong></p>
</blockquote>

<p style="text-align: center;"><a title="timeline" class="" href="{{ site.url }}/timeline/">GO!查看所有文章</a></p>

<ul class="listing">
{% for post in site.posts %}
  {% capture y %}{{post.date | date:"%Y"}}{% endcapture %}
  {% capture this_month %}{{ post.date | date: "%B" }}{% endcapture %}
  {% if year != y %}
    {% assign year = y %}
    <li class="listing-seperator"><h2>{{ y }}</h2></li>
  {% endif %}
  {% if month != this_month %}
    {% assign month = this_month %}
    <li class="month-seperator">{{ this_month }}</li>
  {% endif %}
  <li class="listing-item">
    <time datetime="{{ post.date | date:"%Y-%m-%d" }}">{{ post.date | date:"%Y-%m-%d" }}</time>
    <a href="{{ site.url }}{{ post.url }}" title="{{ post.title }}">{{ post.title }}</a>
  </li>
{% endfor %}
</ul>

