---
layout: page
title: Offensive Security
permalink: /offensive-security/
category: offensive
---

{% for post in site.posts %}
  {% if post.category contains 'offensive' %}
  - [{{ post.title }}]({{ post.url }}) - {{ post.date | date: "%B %d, %Y" }}
  {% endif %}
{% endfor %}
