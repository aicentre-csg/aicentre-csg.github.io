---
title: "AI Centre@CSG - Publications"
layout: gridlay
excerpt: "AI Centre@CSG -- Publications."
sitemap: false
permalink: /publications/
---


# Publications


<ul>
{% for publi in site.data.publications_3 %}
  <li>
    {{ publi.title }} <br />
    <em>{{ publi.authors }}</em> <br />
    <a href="{{ publi.url }}">View Paper</a>
  </li>
{% endfor %}
</ul>
<b>[update in progress]
