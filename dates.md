---
layout: page
title: Dates
---

<div>
  {% assign last_date = '' %}
  {% assign posts = site.posts %}
  {% for post in posts %}
    {% capture this_date %}{{ post.date | date: '%Y-%m' }}{% endcapture %}
    {% if last_date != this_date %}
      <h3>{{this_date}}</h3>
      {% assign last_date = this_date %}
    {% endif %}
    <a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a><br>
  {% endfor %}
</div>
