---
title: "Quentin Fortier"
layout: splash
sticky: false
header:
  overlay_color: "#000"
  overlay_filter: "0.5"
  overlay_image: /assets/images/woods.png
excerpt: "Docteur en informatique"  
---

<center>
  <img src="/assets/images/qf.png" style="max-width: 180px; border-radius: 50%;" alt="Quentin Fortier"> 
  <div class="half-line"><br></div>

  <p> 
    Sur ce site, vous trouverez mes cours, projets, travaux de recherche et mon blog <br>
    Centres d'intérêts: algorithmique, combinatoire, machine learning, programmation...
  </p>
</center>

<br>

<h3 class="archive__subtitle">Derniers articles</h3>
{% if paginator %}
  {% assign posts = paginator.posts %}
{% else %}
  {% assign posts = site.posts %}
{% endif %}

{% assign entries_layout = page.entries_layout | default: 'list' %}
<div class="entries-{{ entries_layout }}">
  {% for i in (0..1) %}
  {% assign post = posts[i] %}
    {% include archive-single.html type=entries_layout %}
  {% endfor %}
</div>

![Metrics](https://metrics.lecoq.io/fortierq?template=classic&base.header=0&base.activity=0&base.community=0&base.repositories=0&base.metadata=0&activity=1&activity.limit=5&activity.load=300&activity.days=14&activity.filter=all&activity.visibility=all&activity.timestamps=false&config.timezone=Europe%2FParis)
