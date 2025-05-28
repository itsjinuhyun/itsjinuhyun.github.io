---
layout: page
title: Projects
---

This is where I'll showcase my projects. Each project will include:

- Project description
- Technologies used
- Key learnings
- Links to code repositories or live demos

{% for project in site.projects %}
  <h2><a href="{{ project.url }}">{{ project.title }}</a></h2>
  <p>{{ project.content | strip_html | truncatewords: 50 }}</p>
{% endfor %}

## Coming soon... 