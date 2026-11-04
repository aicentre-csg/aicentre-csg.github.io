---
title: "AI Centre@CSG - Publications"
layout: gridlay
excerpt: "AI Centre@CSG -- Publications."
sitemap: false
permalink: /publications/
---


# Publications


<p>Selected recent publications by AI Centre members (2025 onwards), newest first. The list is compiled from public bibliographic records (OpenAlex) and may be incomplete.</p>

<ul class="publication-list">
{% assign pubs = site.data.publications_3 | sort: "date" | reverse %}
{% for publi in pubs %}
  <li style="margin-bottom: 14px;">
    <strong>{{ publi.title }}</strong> <br />
    <em>{{ publi.authors }}</em> <br />
    {% if publi.journal != "" %}{{ publi.journal }}, {% endif %}{% if publi.date.size > 4 %}{{ publi.date | date: "%B %Y" }}{% else %}{{ publi.year }}{% endif %}
    {% if publi.url != "" %}&middot; <a href="{{ publi.url }}">View Paper</a>{% endif %}
  </li>
{% endfor %}
</ul>
