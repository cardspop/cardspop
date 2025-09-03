---
layout: page
title: Sets
permalink: /sets/
---

Browse curated checklists. More coming soon.

<ul>
{% assign sets = site.data.sets %}
{% for item in sets %}
  {% assign key = item[0] %}
  {% assign data = item[1] %}
  <li>
    <a href="{{ '/sets/' | append: data.slug | append: '/' | relative_url }}">{{ data.year }} {{ data.brand }}</a>
    <span>— {{ data.notes | default: '' }}</span>
  </li>
{% endfor %}
</ul>

