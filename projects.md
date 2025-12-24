---
layout: default
title: Projects
---

# Projects

{% for project in site.projects %}
## [{{ project.title }}]({{ project.url }})

{% if project.excerpt %}
{{ project.excerpt }}
{% else %}
{{ project.content | strip_html | truncatewords: 50 }}
{% endif %}

{% endfor %}
