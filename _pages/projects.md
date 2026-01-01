---
layout: archive
title: "Projects"
permalink: /projects/
author_profile: true
show_category_sidebar: true
---

{% for post in site.portfolio reversed %}
  {% include archive-single.html %}
{% endfor %}
