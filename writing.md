---
layout: page
title: Writing
description: All blog posts
---

{% for post in site.posts %}
<div class="writing-entry animate-enter" style="animation-delay: {{ forloop.index | times: 0.05 }}s;">
    <a href="{{ post.url | relative_url }}" class="writing-title">{{ post.title }}</a>
    <div class="writing-meta">
        <span>{{ post.content | number_of_words | divided_by: 200 | at_least: 1 }} min read</span>
        <span>{{ post.date | date: "%B %d, %Y" }}</span>
    </div>
    {% if post.description %}
    <p class="writing-desc">{{ post.description }}</p>
    {% endif %}
    {% if post.tags.size > 0 %}
    <div class="writing-tags">
        {% for tag in post.tags %}
        <span class="tag">#{{ tag }}</span>
        {% endfor %}
    </div>
    {% endif %}
</div>
{% endfor %}
