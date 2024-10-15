---
layout: default
---

## Last Posts

<ul>
  {% for post in site.posts %}
    <li>
      <h2><a href="{{ post.url | prepend: site.baseurl }}">{{ post.title }}</a></h2>
      {{ post.excerpt }}
    </li>
  {% endfor %}
</ul>
