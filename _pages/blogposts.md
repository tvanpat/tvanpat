---
layout: archive
permalink: /blog-posts/
title: "Blog Posts"
author_profile: true
header:
  overlay_image:  "/images/andrew-preble-zrhXd14ICRo-unsplash.jpg"
  overlay_filter: 0.25
  caption: "Photo credit: [**Andrew Preble**](https://unsplash.com/photos/zrhXd14ICRo)"
---

{% include group-by-array collection=site.posts field="tags" %}

{% for tag in group_names %}
  {% assign posts = group_items[forloop.index0] %}
  <h2 id="{{ tag | slugify }}" class="archive__subtitle">{{ tag }}</h2>
  {% for post in posts %}
    {% include archive-single.html %}
  {% endfor %}
{% endfor %}
