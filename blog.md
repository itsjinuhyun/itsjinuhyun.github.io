---
layout: default
title: Blog
---

# Blog

{% for post in site.blog %}
### [{{ post.title }}]({{ post.url }})

{{ post.content | strip_html | truncatewords: 30 }}

[Read more]({{ post.url }})

---
{% endfor %}

{% if site.blog.size == 0 %}
### Coming Soon
Check back soon for blog posts!
{% endif %} 