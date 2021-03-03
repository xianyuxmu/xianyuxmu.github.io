---
title: 商业与管理
layout: page
permalink: /business/
---

<ul class="listing">
{% for post in site.posts %}
  {% if (post.categories contains 'Business' || post.categories contains 'Work') %}
  {% else %}
    {% continue %}
  {% endif %}
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

