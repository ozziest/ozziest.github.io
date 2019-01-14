---
layout: default
title: Posts By English Category
---

{% include sections.html %}

<div class="row">
  <div class="col-12">
    <h1 class="category">English</h1>
    <div class="posts">
      {% for post in site.categories.English %}
        {% include article.html %}
      {% endfor %}
    </div>
  </div>
</div>

