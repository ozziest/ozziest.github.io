---
layout: default
title: Posts By Years
---

{% include sections.html %}

<div class="row">
  <div class="col-12">
    <h1 class="category">2018</h1>
    <div class="posts">
      {% for post in site.posts %}
        {% capture year %}{{ post.date | date: "%Y" }}{% endcapture %}
        {% if year == "2018" %}
          {% include article.html %}
        {% endif %}
      {% endfor %}
    </div>
    <h1 class="category">2017</h1>
    <div class="posts">
      {% for post in site.posts %}
        {% capture year %}{{ post.date | date: "%Y" }}{% endcapture %}
        {% if year == "2017" %}
          {% include article.html %}
        {% endif %}
      {% endfor %}
    </div>
    <h1 class="category">2016</h1>
    <div class="posts">
      {% for post in site.posts %}
        {% capture year %}{{ post.date | date: "%Y" }}{% endcapture %}
        {% if year == "2016" %}
          {% include article.html %}
        {% endif %}
      {% endfor %}
    </div>
    <h1 class="category">2015</h1>
    <div class="posts">
      {% for post in site.posts %}
        {% capture year %}{{ post.date | date: "%Y" }}{% endcapture %}
        {% if year == "2015" %}
          {% include article.html %}
        {% endif %}
      {% endfor %}
    </div>
    <h1 class="category">2014</h1>
    <div class="posts">
      {% for post in site.posts %}
        {% capture year %}{{ post.date | date: "%Y" }}{% endcapture %}
        {% if year == "2014" %}
          {% include article.html %}
        {% endif %}
      {% endfor %}
    </div>
    <h1 class="category">2013</h1>
    <div class="posts">
      {% for post in site.posts %}
        {% capture year %}{{ post.date | date: "%Y" }}{% endcapture %}
        {% if year == "2013" %}
          {% include article.html %}
        {% endif %}
      {% endfor %}
    </div>
    <h1 class="category">2012</h1>
    <div class="posts">
      {% for post in site.posts %}
        {% capture year %}{{ post.date | date: "%Y" }}{% endcapture %}
        {% if year == "2012" %}
          {% include article.html %}
        {% endif %}
      {% endfor %}
    </div>
  </div>
</div>

