---
layout: archive
permalink: /archive/
title: "Archive"
author_profile: true
comments: false
header:
  image: /assets/images/texture-feature-01.jpg
  caption: "Photo credit: [**Texture Lovers**](http://texturelovers.com)"
---

{% capture written_year %}'None'{% endcapture %}
{% for post in site.posts %}
  {% capture year %}{{ post.date | date: '%Y' }}{% endcapture %}
  {% if year > "2008" %}
    {% if year != written_year %}
<h2 id="{{ year | slugify }}" class="archive__subtitle">{{ year }}</h2>
      {% capture written_year %}{{ year }}{% endcapture %}
    {% endif %}
    {% include archive-single.html %}
  {% endif %}
{% endfor %}