---
layout: page
permalink: /work/
title: work
description: "A detailed timeline of my professional work experience."
nav: true
nav_order: 5 # This places it after "CV" in the navigation
---

<div class="cv">
{% for entry in site.data.work %}
  <a class="anchor" id="{{ entry.title | slugify }}"></a>
  <div class="card mt-3 p-3">
    <h3 class="card-title font-weight-medium">{{ entry.title }}</h3>
    <div>
      {% if entry.type == 'time_table' %}
        {% include cv/time_table.liquid %}
      {% else %}
        {{ entry.contents }}
      {% endif %}
    </div>
  </div>
{% endfor %}
</div>