---
layout: default
title: Projects
---

<header class="projects-header">
  <h1 class="projects-header-title">Projects</h1>
  <p class="projects-header-subtitle">Research implementations, tools, and papers</p>
</header>

<section class="projects-grid">
  {% for project in site.data.projects %}
  <article class="project-card">
    <h2 class="project-name">{{ project.name }}</h2>
    <p class="project-description">{{ project.description }}</p>
    <div class="project-tech-list">
      {% for tech in project.tech %}
      <span class="project-tech-tag">{{ tech }}</span>
      {% endfor %}
    </div>
    <div class="project-link-row">
      <a href="{{ project.url }}" class="project-link" target="_blank" rel="noopener">View Project →</a>
    </div>
  </article>
  {% endfor %}
</section>
