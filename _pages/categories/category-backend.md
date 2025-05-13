---
title: "Backend"
layout: archive
permalink: /categories/backend/
author_profile: true
sidebar:
  nav: "categories"
---

{% assign posts = site.categories.Backend %}
{% for post in posts %}
  {% include archive-single.html type=page.entries_layout %}
{% endfor %} 