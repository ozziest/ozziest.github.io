---
layout: default
title: Posts By Categories
---

{% include sections.html %}

<div class="row">
  <div class="col-12">
      {% for category in site.categories %}
      <article class="post">
        <h2 class="post-title">
          <a href="/categories/{{ category[0] | downcase }}">
            {{ category[0] }} ({{ category | last | size }})
          </a>
        </h2>
      </article>      
      {% endfor %}
  </div>
</div>

