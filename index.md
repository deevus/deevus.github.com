---
layout: page
title: Programming in Windows et al.
---
{% include JB/setup %}

{% for post in site.posts limit:4 %}
  <div>
    <h1>
        <a href="{{ post.url }}" rel="bookmark" title="Permanent link to ">{{ post.title }}</a>
    </h1>
    <span>{{ post.date | date: '%B' }} {{ post.date | date: '%e' }}, {{ post.date | date: '%Y' }}</span>
    <p>
        {{ post.content }}
    </p>
    <h2>
        <a href="{{ post.url }}" rel="bookmark">Discuss...</a>
    </h2>
  </div>
{% endfor %}