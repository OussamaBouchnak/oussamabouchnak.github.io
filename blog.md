---
layout: default
title: Writing
---

{% assign sorted_posts = site.posts | sort: 'date' | reverse %}
{% assign posts_by_year = sorted_posts | group_by_exp: "post", "post.date | date: '%Y'" %}

<header class="blog-header">
  <h1 class="blog-header-title">Writing</h1>
  <p class="blog-header-subtitle">All articles, research notes and tutorials</p>
</header>

<section class="blog-toc">
  {% for year_group in posts_by_year %}
  <div class="toc-year-group">
    <div class="toc-year">{{ year_group.name }}</div>
    {% for post in year_group.items %}
    <div class="toc-row">
      <span class="toc-title"><a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a></span>
      <span class="toc-dots"></span>
      {% if post.category %}
      <span class="category-tag category-tag-compact">{{ post.category }}</span>
      {% endif %}
      <span class="toc-date">{{ post.date | date: "%b %d" }}</span>
    </div>
    {% endfor %}
  </div>
  {% endfor %}
</section>
