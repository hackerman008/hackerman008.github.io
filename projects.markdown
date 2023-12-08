---
layout: page
title: Projects
permalink: /projects/
category: projects
---

{% for post in site.posts %}
  {% if post.category contains 'projects' %}
  - [{{ post.title }}]({{ post.url }}) - {{ post.date | date: "%B %d, %Y" }}
  {% endif %}
{% endfor %}
