---
layout: page
title: Welcome
---
{% include JB/setup %}

<ul>
  {% assign posts_collate = site.posts %}  
  {% include JB/posts_collate %}
</ul>