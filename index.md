---
layout: page
title: Simon Hartcher - Programming in Windows et al.
---
{% include JB/setup %}

{% for post in site.posts limit:4 %}
  <h1>
      <a href="{{ post.url }}" rel="bookmark" title="Permanent link to ">{{ post.title }}</a>
  </h1>
  <span>{{ post.date | date: '%B' }} {{ post.date | date: '%e' }}, {{ post.date | date: '%Y' }}</span>
  <p>
      {{ post.content }}
  </p>
  <h2>
      <a href="{{ post.url }}#disqus_thread" rel="bookmark"></a>
  </h2>
{% endfor %}

<script type="text/javascript">
  /* * * CONFIGURATION VARIABLES: EDIT BEFORE PASTING INTO YOUR WEBPAGE * * */
  var disqus_shortname = 'simonhartcher'; // required: replace example with your forum shortname

  /* * * DON'T EDIT BELOW THIS LINE * * */
  (function () {
      var s = document.createElement('script'); s.async = true;
      s.type = 'text/javascript';
      s.src = 'http://' + disqus_shortname + '.disqus.com/count.js';
      (document.getElementsByTagName('HEAD')[0] || document.getElementsByTagName('BODY')[0]).appendChild(s);
  }());
</script>