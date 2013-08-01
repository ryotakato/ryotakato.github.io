---
layout: page
---

{% for post in site.posts limit:1 %}
<div id="entry-content">
  <h3>{{ post.title }}</h3>
  <h5>{{ post.date | date: '%Y-%m-%d %H:%M:%S' }}</h5>
  <br/>
  <div>{{ post.content }}</div>
  <br/>
</div>
<hr/>
{% endfor %}


