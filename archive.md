---
layout: page
title: Archive
---

<section>
  {% if site.posts.size > 0 %}
    
    {% capture currentyear %}2024{% endcapture %} 
    {% assign firstpostyear = site.posts[0].date | date: '%Y' %}
    
    {% if currentyear == firstpostyear %}
        <h3>This year's posts</h3>
    {% else %}  
        <h3>{{ firstpostyear }}</h3>
    {% endif %}

    <ul>
    {% for post in site.posts %}
      {% capture year %}{{ post.date | date: '%Y' }}{% endcapture %}
      {% unless post.next %}
        {% if year == currentyear %}
          <li><time>{{ post.date | date:"%d %b" }} - </time>
            <a href="{{ post.url | prepend: site.baseurl | replace: '//', '/' }}">
              {{ post.title }}
            </a>
          </li>
        {% endif %}
      {% else %}
        {% capture nyear %}{{ post.next.date | date: '%Y' }}{% endcapture %}
        {% if year != nyear %}
          {% if year == currentyear %}
            <li><time>{{ post.date | date:"%d %b" }} - </time>
              <a href="{{ post.url | prepend: site.baseurl | replace: '//', '/' }}">
                {{ post.title }}
              </a>
            </li>
          {% endif %}
        {% endif %}
      {% endunless %}
    {% endfor %}
    </ul>
    
  {% else %}
    <h3>No posts available.</h3>
  {% endif %}
</section>
