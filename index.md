---
layout: page
---
{% include JB/setup %}

{% for page in site.posts limit:4 %}
  {% include _layouts/post %}
{% endfor %}