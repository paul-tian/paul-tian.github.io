---
layout: page
title: Links
description: 
keywords: 
comments: true
menu: Links
permalink: /links/
---


> Here are some blogs by other coders.

<ul>
{% for link in site.data.links %}
  {% if link.src == 'blog' %}
  <li><a href="{{ link.url }}" target="_blank">{{ link.name}}</a></li>
  {% endif %}
{% endfor %}
</ul>


> Friendly Links

<ul>
{% for link in site.data.links %}
  {% if link.src == 'www' %}
  <li><a href="{{ link.url }}" target="_blank">{{ link.name}}</a></li>
  {% endif %}
{% endfor %}
</ul>


> Those who I respect

<ul>
{% for link in site.data.links %}
  {% if link.src == 'respect' %}
  <li><a href="{{ link.url }}" target="_blank">{{ link.name}}</a></li>
  {% endif %}
{% endfor %}
</ul>