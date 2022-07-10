---
title: "extension"
permalink: extension
layout: archive

author-profile: true
sidebar:
  nav: "docs"
---

{% assing posts = site.categories.extension %}
{% for post in posts %}
  {% include custom-archive-single.html type=entries_layout %}
{% endfor %}