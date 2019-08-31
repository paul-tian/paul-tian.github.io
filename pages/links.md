---
layout: page
title: Links
description: 
keywords: 
comments: true
menu: Links
permalink: /links/
---

> This is where we connect.

{% for link in site.data.links %}
  {% if link.src == 'blog' %}
* [{{ link.name }}]({{ link.url }})
  {% endif %}
{% endfor %}
<br>

> Friendly Links
> 
{% for link in site.data.links %}
  {% if link.src == 'www' %}
* [{{ link.name }}]({{ link.url }})
  {% endif %}
{% endfor %}
<br>

> Those who I respect

{% for link in site.data.links %}
  {% if link.src == 'respect' %}
* [{{ link.name }}]({{ link.url }})
  {% endif %}
{% endfor %}
<br>