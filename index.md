---
layout: default
title: Oskar Grabowski
---

# Hi, I'm Oskar 👋

This is my personal website. From time to time I'll be posting here —
about things I'm working on and things I find interesting.

The site is under construction; for now I'm just checking that everything works. 🚧

## Blog

{% if site.posts.size > 0 %}
<ul class="post-list">
  {% for post in site.posts %}
  <li>
    <time datetime="{{ post.date | date_to_xmlschema }}">{{ post.date | date: "%Y-%m-%d" }}</time>
    <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
  </li>
  {% endfor %}
</ul>
{% else %}
<p class="muted">Nothing here yet — first post coming soon.</p>
{% endif %}

## Contact

- Email: [oskar.grabowski@gmail.com](mailto:oskar.grabowski@gmail.com)
