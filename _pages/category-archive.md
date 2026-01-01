---
layout: archive
permalink: /categories/:category/
author_profile: true
sidebar:
  nav: "categories"
---

{% assign category = page.url | split: "/" | last %}
{% assign posts = site.categories[category] %}

<h1 class="page__title">{{ category }}</h1>

{% for post in posts %}
  {% include archive-single.html %}
{% endfor %}
