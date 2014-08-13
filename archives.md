---
layout: page
title: Archives
permalink: /archives/
---

<div class="archives">
{% for category in site.categories %}
  <h3 class="category">{{ category | first | upcase }}</h3>
  <ul class="blogs">
    {% for posts in category %}
      {% for post in posts %}
          {% if post.title %}
          <li><a href="{{ post.url | prepend: site.baseurl }}" class="post-link">{{ post.title }}</a></li>
          {% endif %}
      {% endfor %}
    {% endfor %}
  </ul>
{% endfor %}
</div>
