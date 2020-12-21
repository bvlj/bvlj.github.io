---
layout: collection
title: Blog
sitemap:
    priority: 0.9
---

<section>
{% for post in site.posts %}
    <article>
        <a href="{{ post.url | prepend: site.baseurl }}">
            <h3>{{ post.title }}</h3>
            <p class="subtitle">{{ post.date | date: "%Y %B %d" }}</p>
        </a>
    </article>
{% endfor %}
</section>
