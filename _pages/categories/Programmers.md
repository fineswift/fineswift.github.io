---
# title: "Programmers Algorithm Solutions"
permalink: Programmers/
layout: archive

author-profile: true
sidebar:
  nav: "docs"
---

{% assign posts = site.categories.Programmers %}
{% for post in posts %}
  {% include archive-single.html type=entries_layout %}
{% endfor %}