---
layout: page
title: Archive
---

<section>
  {% if site.posts[0] %}

    <!-- Especifica manualmente el año aquí -->
    <h3>2024</h3>

    {%for post in site.posts %}
      {% unless post.next %}
        <ul>
      {% else %}
        <!-- Cambia manualmente el año donde lo necesites -->
        <h3>2024</h3>
        <ul>
      {% endunless %}
        <li><time>04 Oct - </time>
          <a href="{{ post.url | prepend: site.baseurl | replace: '//', '/' }}">
            {{ post.title }}
          </a>
        </li>
    {% endfor %}
    </ul>

  {% endif %}
</section>
