---
layout: default
title: Projects
---

# Projects

{% for project in site.projects %}
  <h2><a href="{{ project.url }}">{{ project.title }}</a></h2>
  {% if project.excerpt %}
    <p>{{ project.excerpt }}</p>
  {% else %}
    <p>{{ project.content | strip_html | truncatewords: 50 }}</p>
  {% endif %}
{% endfor %}
