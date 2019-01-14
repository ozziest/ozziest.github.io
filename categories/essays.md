---
layout: default
title: Posts By Essay Category
---

{% include sections.html %}

<div class="row">
  <div class="col-12">
    <h1 class="category">Essays</h1>
    <div class="posts">
      {% for post in site.categories.Essays %}
        {% include article.html %}
      {% endfor %}
    </div>
  </div>
</div>

