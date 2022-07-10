---
title: "programmers"
permalink: programmers
layout: archive

author-profile: true
sidebar:
  nav: "docs"
---

{% assign posts = site.categories.programmers %}
{% for post in posts %}
  {% include custom-archive-single.html type=entries_layout %}
{% endfor %}