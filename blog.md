---
layout: default
title: Blog
---

# Blog

This is where I share my thoughts on various topics that interest me.

## Latest Posts

{% for post in site.blog %}
### [{{ post.title }}]({{ post.url }})
*{{ post.date | date: "%B %d, %Y" }}*

{{ post.content | strip_html | truncatewords: 30 }}

[Read more]({{ post.url }})

---
{% endfor %}

{% if site.blog.size == 0 %}
### Coming Soon
Check back soon for blog posts!
{% endif %} 