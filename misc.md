---
layout: default
title: Miscellaneous
---

# Miscellaneous

This is a space for random creative expressions, ideas, and experiments that don't fit neatly into other categories.

## Creative Expressions

{% for item in site.misc %}
### [{{ item.title }}]({{ item.url }})
*{{ item.date | date: "%B %d, %Y" }}*
{% if item.type %}*Type: {{ item.type | join: ", " }}*{% endif %}

{{ item.content | strip_html | truncatewords: 30 }}

[View full piece]({{ item.url }})

---
{% endfor %}

{% if site.misc.size == 0 %}
### Coming Soon
Check back soon for creative expressions!
{% endif %}

---

This page is constantly evolving as I explore different creative outlets and interests. 