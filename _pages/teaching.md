---
toc: true
toc_label: "Cours"
toc_sticky: true
title: "Cours"
permalink: /teaching/
courses:
  - name: Recherche opérationnelle à l'[ENTPE](https://www.entpe.fr/)
    url: /assets/teaching/ENTPE/
    items:
      - name: "Arbre couvrant de poids minimal"
        url: arbre_couvrant.pdf
      - name: "Ordonnancement"
        url: ordonnancement.pdf
      - raw: "[Exo 13 corrigé](/assets/teaching/ENTPE/exo_13.png). Exo 11 corrigé sur GeoGebra : [MPM](https://www.geogebra.org/geometry/pvytdupg), [PERT à compléter](https://www.geogebra.org/geometry/u7z9jngn)"
      - raw: "Programmation linéaire: [résolution d'un PL simple](/assets/teaching/ENTPE/lp_ex.html) ([représentation avec Geogebra](https://www.geogebra.org/m/jcjnzg9x)), [résolution avec Python (exercice Roulements à bille)](https://github.com/fortierq/ENTPE/blob/master/lp/roulement_billes.ipynb), [simplexe à 2 phases](/assets/teaching/ENTPE/simplexe_2_phases.html)"
---

## [Informatique en MPI à la Martinière Monplaisir](https://mpi-lamartin.github.io/mpi-info)

## [Option informatique en MP](https://mp-info.github.io)

## Informatique commune en CPGE (MPSI, PCSI, PTSI)

- [Cours d'informatique commune, 1ère année](https://cpge-itc.github.io/itc1)
- [Cours d'informatique commune, 2ème année](https://cpge-itc.github.io/itc2)

## [Informatique en BCPST, 2ème année](https://cpge-itc.github.io/bcpst2)

## [Informatique en MP2I](https://mp2i-info.github.io)

## [Cours et exercices interactifs en SQL](https://sql-exercices.github.io)

## [Optimisation en Master Intelligence Artificielle Distribuée (Université de Paris)](https://fortierq.github.io/oc-m1-2021)

{% for course in page.courses %}

## {{ course.name }}

{% if course.chapters %}
{% for chapter in course.chapters %}

- ### {{ chapter.name }}
  {% for lecture in chapter.lectures %}
  - [Cours {{ forloop.index }} : {{ lecture.name }}]({{ course.url | append: lecture.url }})
    {% endfor %}
  - Exercices : {% for exo in chapter.exos %} [{{ exo.name }}]({{ course.url | append: exo.url }}) {% endfor %}
    {% endfor %}
    {% elsif course.lectures %}
    {% for lecture in course.lectures %}
  - [Cours {{ forloop.index }} : {{ lecture.name }}]({{ course.url | append: lecture.url }})
    {% endfor %}  
    {% else %}
    {% for e in course.items %}
  - {% if e.raw %} {{ e.raw }}
    {% else %}
    [{{ e.name }}]({{ course.url | append: e.url }})
    {% if e.cor %} [{{ e.cor }}]({{ course.url | append: e.cor_url }}) {% endif %}
    {% endif %}
    {% endfor %}  
    {% endif %}
    {% endfor %}
