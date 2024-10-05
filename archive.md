---
layout: page
title: Archive
---

<section>
  {% if site.posts[0] %}

    <!-- Especifica manualmente el año aquí -->
    <h3>2024</h3>

    <ul>
      <!-- Aquí pones manualmente la fecha del post -->
      <li><time>4 Oct - </time>
        <a href="{{ site.baseurl }}/post-url-1">
          Título del Post 1
        </a>
      </li>
    </ul>


  {% endif %}
</section>
