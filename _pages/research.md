---
permalink: /research/
title: "Recherche"
date: now
toc: true
toc_label: "Recherche"
toc_sticky: true
publications:
  - name: "On packing spanning arborescences with matroid constraint"
    url: https://onlinelibrary.wiley.com/doi/full/10.1002/jgt.22484
    journal: "Journal of Graph Theory"
    date: 2020
  - name: "Old and new results on packing arborescences in directed hypergraphs"
    url: https://www.sciencedirect.com/science/article/abs/pii/S0166218X17305085
    journal: "Discrete Applied Mathematics" 
    date: 2018
  - name: "Defensive Leakage Camouflage"
    url: https://link.springer.com/chapter/10.1007/978-3-642-37288-9_19
    journal: "CARDIS 2012: Smart Card Research and Advanced Applications" 
    date: 2012
talks:
  - name: "Séminaire des doctorants"
    url: http://lmb.univ-fcomte.fr/Connexite-avec-contraintes-de
    date: 2018
    place: "Laboratoire de Mathématiques de Besançon"
  - name: "Journées Graphes et Algorithmes"
    url: https://jga2016.github.io
    date: 2016
    place: "LAMSADE, Université Paris-Dauphine"
  - name: "Journées Polyèdres et Optimisation Combinatoire"
    url: https://www.univ-lehavre.fr/spip.php?article418
    date: 2015
    place: "Université du Havre"
  - name: "Journées Graphes et Algorithmes"
    url: https://www.gdr-im.fr/event/journees-graphes-et-algorithmes-12-14-novembre-2014-a-dijon
    date: 2014
    place: "Université de Bourgogne"
---

## Thèse
**Aspects of connectivity with matroid constraints in graphs**
{: style="text-align: center;"}

The notion of connectivity is fundamental in graph theory. We study a recent development in this field, with the addition of matroid constraints. We extend Menger's and Edmonds' packing theorems in this new theory. By finding a counterexample with more than 300 vertices, we disprove a recent conjecture from András Frank.
{: .notice--primary}

[**Manuscrit**](https://tel.archives-ouvertes.fr/tel-01838231){: .btn .btn--primary}  [**Soutenance**](/assets/soutenance.pdf){: .btn .btn--primary}  
{: style="text-align: center;"}

## Publications
- Rapporteur pour *Journal of Graph Theory*
{% for pub in page.publications %}
- [{{ pub.name }}]({{ pub.url }})  
*{{ pub.journal }}*, {{ pub.date }}
{% endfor %}

## Présentations

{% for talk in page.talks %}
- [{{ talk.name }}]({{ talk.url }})  
*{{ talk.place }}*, {{ talk.date }}
{% endfor %}
