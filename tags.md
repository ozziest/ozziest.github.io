---
layout: default
title: Tags
---

{% include sections.html %}

<div class="row">
  <div class="col-12">
      {% for tag in site.tags %}
      <article class="post">
        <h2 class="post-title">
          <a href="/tags/{{ tag[0] | downcase }}">
            {{ tag[0] }} ({{ tag | last | size }})
          </a>
        </h2>
      </article>      
      {% endfor %}
  </div>
</div>

