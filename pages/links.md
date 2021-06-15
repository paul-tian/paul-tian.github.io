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