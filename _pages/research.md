---
title: "AI Centre - Areas of Focus"
layout: textlay
excerpt: "AI Centre@CSG - Research areas and representative works"
sitemap: false
permalink: /research/
---

# Areas of Focus

At the AI Centre @ City St George’s, our research covers a broad spectrum of artificial intelligence — from **core theory** to **applied domains**.  
We are united by a shared goal: to create interpretable, robust, and human-centred AI systems.  

---

{% for area in site.data.research %}
<div class="research-area" style="margin-bottom: 3em;">

  <h2>{{ area.title }}</h2>
  {% if area.image %}
  <img src="{{ site.url }}{{ site.baseurl }}/images/researchpic/{{ area.image }}"
       alt="{{ area.title }}"
       style="width: 60%; margin: 15px 0; border-radius: 10px;">
  {% endif %}

  <p>{{ area.description }}</p>

  {% if area.papers %}
  <h4>Representative Publications:</h4>
  <ul>
    {% for paper in area.papers %}
    <li>
      <strong><a href="{{ paper.link }}" target="_blank">{{ paper.title }}</a></strong><br>
      <em>{{ paper.authors }}</em>
    </li>
    {% endfor %}
  </ul>
  {% endif %}

  <hr>
</div>
{% endfor %}
