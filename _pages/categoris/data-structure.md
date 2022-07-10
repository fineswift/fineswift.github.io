---
title: "data-structure"
permalink: data-structure
layout: archive

author-profile: true
sidebar:
  nav: "docs"
---

{% assing posts = site.categories.data-structure %}
{% for post in posts %}
  {% include custom-archive-single.html type=entries_layout %}
{% endfor %}