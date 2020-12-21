---
layout: page
title: About
permalink: /about/
---

# Education

Student at [USI](https://usi.ch) (Lugano - CH)

# Contact info

## Mail

<ul>
{% for email in site.author.email %}
    <li><a href="#">{{ email }}</a></li>
{% endfor %}
</ul>

## Other

* Github: [@{{ site.author.github }}](https://github.com/{{ site.author.github }})
* LineageOS: [@{{ site.author.lineage.name }}](https://review.lineageos.org/q/owner:{{ site.author.lineage.email }})
