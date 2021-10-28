---
layout: page
---

{% for post in site.posts limit:1 %}
<div id="entry-content">
  <h3><a herf="{{ post.url }}" style="color: unset;">{{ post.title }}</a></h3>
  <h5>{{ post.date | date: '%Y-%m-%d %H:%M:%S' }}</h5>
  <ul style="text-align: right;" class="ul-none">
    <li style="display:inline-block;">Tags:</li>
    {% assign tags_list = post.tags %}
    {% if tags_list.first[0] == null %}
      {% for tag in tags_list %} 
        <li style="display:inline-block;"><a href="{{ BASE_PATH }}{{ site.path.tags }}#{{ tag }}-ref">{{ tag }}</a></li>
      {% endfor %}
    {% else %}
      {% for tag in tags_list %} 
        <li style="display:inline-block;"><a href="{{ BASE_PATH }}{{ site.path.tags }}#{{ tag[0] }}-ref">{{ tag[0] }}</a></li>
      {% endfor %}
    {% endif %}
    {% assign tags_list = nil %}
  </ul>
  <br/>
  <div id="entry-body">{{ post.content }}</div>
  <br/>
</div>
<hr/>
ここはTOPページです。コメントなどは<a herf="{{ post.url }}">個別のページ</a>へお願いします。
{% endfor %}


