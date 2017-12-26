---
layout:       page
title:        Tags
permalink:    /tags
---

{% assign all_tags = Nil %}
{% for post in site.posts %}
  {% assign all_tags = all_tags | concat: post.tags %}  
{% endfor %}
{% assign all_tags = all_tags | join: ',' %}
{% assign all_tags = all_tags | split: ',' %}
{% assign all_tags = all_tags | uniq | sort %}
{% for tag in all_tags %}
  {% include tag.html %}
{% endfor %}
