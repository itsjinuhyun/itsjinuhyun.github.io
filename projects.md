---
layout: default
title: Projects
---

# Projects

{% for project in site.projects %}
  <h2><a href="{{ project.url }}">{{ project.title }}</a></h2>
  <p>{{ project.content | strip_html | truncatewords: 50 }}</p>
{% endfor %}
