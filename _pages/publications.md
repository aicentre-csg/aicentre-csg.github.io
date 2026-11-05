---
title: "AI Centre@CSG - Publications"
layout: gridlay
excerpt: "AI Centre@CSG -- Publications."
sitemap: false
permalink: /publications/
---


# Publications


<p>Selected publications by AI Centre members, grouped by year, newest first. The list is compiled from public bibliographic records (OpenAlex) and may be incomplete.</p>

{% assign pubs = site.data.publications_3 | sort: "date" | reverse %}
{% assign current_year = "" %}
{% for publi in pubs %}
  {% assign y = publi.year | default: publi.date | slice: 0, 4 %}
  {% if y != current_year %}
    {% unless forloop.first %}</ul>{% endunless %}
    {% assign current_year = y %}
<h2 id="{{ y }}" style="margin-top: 1.5em;">{{ y }}</h2>
<ul class="publication-list">
  {% endif %}
  <li style="margin-bottom: 14px;">
    <strong>{{ publi.title }}</strong> <br />
    <em>{{ publi.authors }}</em> <br />
    {% if publi.journal != "" %}{{ publi.journal }}, {% endif %}{% if publi.date.size > 4 %}{{ publi.date | date: "%B %Y" }}{% else %}{{ publi.year }}{% endif %}
    {% if publi.url != "" %}&middot; <a href="{{ publi.url }}">View Paper</a>{% endif %}
  </li>
{% endfor %}
</ul>
