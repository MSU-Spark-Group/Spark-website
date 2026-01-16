---
title: Publication
nav:
  order: 3
  tooltip: Software, datasets, and more
---

# {% include icon.html icon="fa-solid fa-wrench" %}Publication

{% include tags.html tags="journal papers, presentations, posters" %}

{% include search-info.html %}

{% include search-box.html %}

{% include tags.html tags="journal papers, presentations, posters" %}

{% include section.html %}

## Featured

{% include list.html component="card" data="projects" filter="group == 'featured'" %}

{% include section.html %}

## More

{% include list.html component="card" data="projects" filter="!group" style="small" %}
