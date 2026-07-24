---
layout: page
title: "Teaching a Drone to See"
description: A six-part sequence on multi-resolution UAV imagery, instance segmentation, canopy measurement, and deployment.
permalink: /sequences/uav-thesis.html
---

This sequence follows an applied computer vision thesis from the first failure at a new flight altitude through dataset repair, model comparison, physical canopy measurement, and a practical deployment framework.

{% assign sequence_posts = site.posts | where: "series", "uav-thesis" | sort: "series_order" %}
<ol class="sequence-list">
{% for post in sequence_posts %}
  <li>
    <span class="sequence-number">{{ post.series_order | prepend: "0" | slice: -2, 2 }}</span>
    <div>
      <h2><a href="{{ post.url | relative_url }}">{{ post.title }}</a></h2>
      <p>{{ post.description }}</p>
      <span class="meta">{{ post.content | number_of_words | divided_by: 200 | at_least: 1 }} min read</span>
    </div>
  </li>
{% endfor %}
</ol>
