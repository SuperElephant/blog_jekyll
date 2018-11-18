---
layout: page
title: Tags
---

<div>
  {% assign sorted_tags = site.data.tags | sort:"name" %}
  {% for tag in sorted_tags %}
    {% include tag_link_formatter.html tag=tag tag_separator='<br />' %}
  {% endfor %}
</div>


{% comment %}
<div>
    {% assign all_tags = '' %}
    {% for post in site.posts %}
        {% for tag in post.tags %}
            {% capture all_tags%} {{all_tags | append: "," | append: tag}}
            {% endcapture %}
        {% endfor %}
    {% endfor %}

    {% capture all_tags%}
        {{all_tags | remove_first: "," | split: "," | uniq | sort_natural}}
    {% endcapture %}

    {% for tag in all_tags %}
        {% include tag_link_formatter.html tag=tag tag_separator='<br/>' %}
    {% endfor %}
</div>
{% endcomment %}