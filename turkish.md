---
layout: default
title: Posts By Turkish Category
---

{% include sections.html %}

<div class="row">
  <div class="col-12">
    <h1 class="category">Turkish</h1>
    <div class="posts">
      {% for post in site.categories.Turkish %}
        {% include article.html %}
      {% endfor %}
    </div>
  </div>
</div>

