---
layout: collection
title: Projects
sitemap:
    priority: 0.9
---

<section>
{% for project in site.projects %}
    <article>
        <a href="{{ project.url | prepend: site.baseurl }}">
            <h3>{{ project.title }}</h3>
            <p class="subtitle">{{ project.subtitle }}</p>
        </a>
    </article>
{% endfor %}
</section>
