---
layout: archive
title: "Publications"
permalink: /publications/
author_profile: true
classes: wide
---

You can also find my articles on <a href="https://scholar.google.com/citations?user=mbMndyUAAAAJ=en" target="_blank">my Google Scholar profile <i class="fas fa-graduation-cap"></i></a>

{% for post in site.publications reversed %}
  {% include archive-single-publications.html %}
{% endfor %}
