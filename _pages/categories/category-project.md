---
title: "Project"
layout: archive
permalink: /categories/project/
author_profile: true
sidebar:
  nav: "categories"
---

{% assign posts = site.categories.Project %}
{% for post in posts %}
  {% include archive-single.html type=page.entries_layout %}
{% endfor %} 