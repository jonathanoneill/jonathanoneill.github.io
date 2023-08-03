---
layout: archive
permalink: /categories/
title: "Posts by Category"
author_profile: true
---

{% include group-by-array collection=site.posts field="categories" %}

{% for category in group_names %}
  {% assign posts = group_items[forloop.index0] %}
  <h2 id="{{ category | slugify }}" class="archive__subtitle">{{ category }}</h2>
  {% for post in posts %}
    {% capture year %}{{ post.date | date: '%Y' }}{% endcapture %}
    {% if year > "2016" %}
      {% include archive-single.html %}
    {% endif %}
  {% endfor %}
{% endfor %}