---
layout: page
title: Projects
permalink: /projects/
description: A selection of projects spanning AI, MLOps, and Cybersecurity.
nav: true
nav_order: 3
---

<div class="row">
  {% assign sorted_projects = site.projects | sort: 'importance' %}
  {% for project in sorted_projects %}
    <div class="col-md-4 mb-4">
      <div class="card h-100">
        <a href="{{ project.url }}">
          <img src="{{ project.img | relative_url }}" class="card-img-top" alt="{{ project.title }}">
        </a>
        <div class="card-body">
          <h5 class="card-title">
            <a href="{{ project.url }}">{{ project.title }}</a>
          </h5>
          <p class="card-text">{{ project.description }}</p>
        </div>
      </div>
    </div>
  {% endfor %}
</div>
