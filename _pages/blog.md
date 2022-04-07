---
layout: archive
permalink: /blog/
classes: wide
author_profile: false
sidebar:
  nav: "posts_nav"
---

{{ site.data.ui-text[site.locale].recent_posts | default: "Recent Posts" }}
{% if paginator %} {% assign posts = paginator.posts %} {% else %} {% assign posts = site.posts %} {% endif %}

{% assign entries_layout = page.entries_layout | default: 'list' %}

{% for post in posts %} {% include archive-single.html type=entries_layout %} {% endfor %}
{% include paginator.html %}
