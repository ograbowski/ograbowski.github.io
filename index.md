---
layout: default
title: Oskar Grabowski
---

# Cześć, jestem Oskar 👋

To jest moja osobista strona. Od czasu do czasu będę tu publikował posty —
o rzeczach, nad którymi pracuję i które mnie interesują.

Strona jest w budowie; na razie sprawdzam, czy wszystko działa. 🚧

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
<p class="muted">Jeszcze nic tu nie ma — pierwszy post wkrótce.</p>
{% endif %}

## Kontakt

- E-mail: [oskar.grabowski@gmail.com](mailto:oskar.grabowski@gmail.com)
