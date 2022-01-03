---
layout: default
permalink: /programming/
---

# Programming

## [EDX](https://www.edx.org/) (free)
### Data OOP with Java I
{% for post in site.categories.cs1331xI %}
 <li><span>{{ post.date | date_to_string }}</span> &nbsp; <a href="{{ post.url }}">{{ post.title }}</a></li>
{% endfor %}

### Data Structures I
{% for post in site.categories.cs1332xI %}
 <li><span>{{ post.date | date_to_string }}</span> &nbsp; <a href="{{ post.url }}">{{ post.title }}</a></li>
{% endfor %}


## [Pluralsight](https://www.pluralsight.com/) (paid)
### Clean Code - Writing Code for Humans
{% for post in site.categories.clean-code %}
 <li><span>{{ post.date | date_to_string }}</span> &nbsp; <a href="{{ post.url }}">{{ post.title }}</a></li>
{% endfor %}


## [Udemy](https://www.udemy.com) (paid)
### The Complete 2021 Web Development Bootcamp
{% for post in site.categories.webdev %}
 <li><span>{{ post.date | date_to_string }}</span> &nbsp; <a href="{{ post.url }}">{{ post.title }}</a></li>
{% endfor %}

### Web Application Security for Absolute Beginners
{% for post in site.categories.security %}
 <li><span>{{ post.date | date_to_string }}</span> &nbsp; <a href="{{ post.url }}">{{ post.title }}</a></li>
{% endfor %}