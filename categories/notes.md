---
layout: default
title: Posts By Notes Category
---

{% include sections.html %}

<div class="row">
  <div class="col-12">
    <h1 class="category">Notes</h1>
    <div class="posts">
      {% for post in site.categories.Notes %}
        {% include article.html %}
      {% endfor %}
    </div>
  </div>
</div>

