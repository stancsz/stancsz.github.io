---
layout: default
---

# Welcome to My Minimal Blog

This is the homepage of my new blog. Below you can find the latest posts.

<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
      - {{ post.date | date: "%B %d, %Y" }}
    </li>
  {% endfor %}
</ul>
