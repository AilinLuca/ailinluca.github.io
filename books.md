---
layout: default
permalink: /books/
---

# Books

## Nonfiction Summaries
{% for post in site.categories.nonfiction %}
 <li><span>{{ post.date | date_to_string }}</span> &nbsp; <a href="{{ post.url }}">{{ post.title }}</a></li>
{% endfor %}

## Fiction Analysis
{% for post in site.categories.fiction %}
 <li><span>{{ post.date | date_to_string }}</span> &nbsp; <a href="{{ post.url }}">{{ post.title }}</a></li>
{% endfor %}