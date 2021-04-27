---
title: Projets
permalink: /projects/
classes: wide
layout: collection
mathjax: true
feature_row:
  - image_path: assets/images/portfolio/mtgscan.jpg
    title: "MTGScan"
    excerpt: "Package Python permettant de reconnaître visuellement des cartes à l'aide d'un algorithme de reconnaissance de caractères (OCR) et de traitement de chaînes de caractères (fuzzy string matching)."
    url: https://github.com/fortierq/mtgscan
  - image_path: assets/images/portfolio/ocd.jpg
    title: "OC3D"
    excerpt: "Logiciel en C++ pour la découpe optimale de volume 3D, en minimisant l'aire de section. Pour cela, on décompose le volume en tétraèdres puis on applique un algorithme min cost - max flow sur le graphe dual."
    url: https://github.com/fortierq/OC3D
  - image_path: assets/images/portfolio/snt.jpg
    title: "Sciences Numériques et Technologie"
    excerpt: "Livre de Seconde traitant de: programmation Python, internet, réseaux sociaux, géolocalisation, photographie numérique..."
    url: https://www.editions-ellipses.fr/accueil/119-sciences-numeriques-et-technologie-seconde-nouveaux-programmes-9782340033658.html
feature_row2:
  - image_path: assets/images/portfolio/pavage.png
    title: "Pavages"
    excerpt: "Notebook Jupyter autour de problèmes de pavages : résolution par programmation linéaire et petits exercices."
    url: assets/nb/pavage.html
  - image_path: assets/images/portfolio/graphe.png
    title: "Simple Graph Library"
    excerpt: "Bibliothèque C++ de théorie des graphes : arbre couvrant minimal, plus court chemin, flot maximum... Utilisation de la programmation orientée objet et de templates."
    url: https://github.com/fortierq/SGL
  - image_path: assets/images/portfolio/hirsch.png
    title: "Conjecture de Hirsch"
    excerpt: Recherche d'un contre-exemple à la conjecture de Hirsch en utilisant le SMT-solver Z3 en C++. La conjecture de Hirsch affirme que le diamètre d'un polytope de dimension $$d$$ avec $$n$$ faces est inférieur à $$n - d$$.
    url: https://github.com/fortierq/Prismatoid
---

{% include feature_row %}

{% include feature_row id="feature_row2" %}
