---
layout: page
permalink: /teaching/
title: teaching
description: "A summary of my university teaching, professional tutoring, and volunteer instruction roles."
nav: true
nav_order: 6
---

<div class="cv">
{% for entry in site.data.teaching %}
  <a class="anchor" id="{{ entry.title | slugify }}"></a>
  <div class="card mt-3 p-3">
    <h3 class="card-title font-weight-medium">{{ entry.title }}</h3>
    <div>
      {% if entry.type == 'time_table' %}
        {% include cv/time_table.liquid %}
      {% elsif entry.type == 'nested_list' %}
        {% include cv/nested_list.liquid %}
      {% else %}
        {{ entry.contents }}
      {% endif %}
    </div>
  </div>
{% endfor %}
</div>