---
title: 生活与思考
layout: page
permalink: /life/
---

<ul class="listing">
{% for post in site.posts %}
  {% if post.categories contains 'Life' || post.categories contains 'Thinking' %}
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

