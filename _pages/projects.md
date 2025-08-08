---
layout: page
title: Projects
permalink: /projects/
description: A selection of projects spanning AI, MLOps, and Cybersecurity.
nav: true
nav_order: 3
horizontal: false
---

<div class="projects">
  {%- assign sorted_projects = site.projects | sort: "importance" -%}
  
  {%- if page.horizontal -%}
    <div class="container">
      <div class="row row-cols-1 row-cols-md-2">
        {%- for project in sorted_projects -%}
          {% include projects_horizontal.liquid project=project %}
        {%- endfor -%}
      </div>
    </div>
  {%- else -%}
    <div class="row row-cols-1 row-cols-md-3">
      {%- for project in sorted_projects -%}
        {% include projects.liquid project=project %}
      {%- endfor -%}
    </div>
  {%- endif -%}
</div>
