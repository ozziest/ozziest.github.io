---
layout: default
title: Posts By Coding Category
---

{% include sections.html %}

<div class="row">
  <div class="col-12">
    <h1 class="category">Coding</h1>
    <div class="posts">
      {% for post in site.categories.Coding %}
        {% include article.html %}
      {% endfor %}
    </div>
  </div>
</div>

