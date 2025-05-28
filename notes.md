---
layout: default
title: Notes
---

# Notes

This is where I'll share my thoughts, ideas, and learnings on various topics including:

- Data Science concepts
- Mathematical theories
- Programming techniques
- Book summaries
- Research findings

{% for note in site.notes %}
  <h2><a href="{{ note.url }}">{{ note.title }}</a></h2>
  <p>{{ note.date | date: "%B %d, %Y" }}</p>
  <p>{{ note.content | strip_html | truncatewords: 50 }}</p>
{% endfor %}

## Coming soon... 