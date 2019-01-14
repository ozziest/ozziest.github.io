---
layout: default
title: Posts By Presentation Category
---

{% include sections.html %}

<div class="row">
  <div class="col-12">
    <h1 class="category">Presentations</h1>
    <div class="posts">
      {% for post in site.categories.Presentation %}
        {% include article.html %}
      {% endfor %}
    </div>
  </div>
</div>

