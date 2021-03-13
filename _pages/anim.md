---
title: Animations d'algorithmes
permalink: /anim/
classes: wide
layout: collection
mathjax: false
anim: true 
feature_row:
  - image_path: assets/anim/dijkstra.png
    gif_path: assets/anim/dijkstra.gif
    title: "Dijkstra"
    excerpt: "Plus courts chemins depuis un sommet dans un graphe dont les poids sont positifs"
    url: https://github.com/fortierq/mtgscan
  - image_path: assets/images/portfolio/ocd.jpg
    title: "OC3D"
    excerpt: "Logiciel en C++ pour la découpe optimale de volume 3D, en minimisant l'aire de section. Pour cela, on décompose le volume en tétraèdres puis on applique un algorithme min cost - max flow sur le graphe dual."
    url: https://github.com/fortierq/OC3D
  - image_path: assets/images/portfolio/snt.jpg
    alt: "Sciences Numériques et Technologie"
    title: "Sciences Numériques et Technologie"
    excerpt: "Livre de Seconde traitant de: programmation Python, internet, réseaux sociaux, géolocalisation, photographie numérique..."
    url: https://www.editions-ellipses.fr/accueil/119-sciences-numeriques-et-technologie-seconde-nouveaux-programmes-9782340033658.html
feature_row2:
  - image_path: assets/images/portfolio/pavage.png
    title: "Pavages"
    excerpt: "Notebook Jupyter autour de problèmes de pavages : résolution par programmation linéaire et petits exercices."
    url: https://github.com/fortierq/Pavage
  - image_path: assets/images/portfolio/graphe.png
    title: "Simple Graph Library"
    excerpt: "Bibliothèque C++ de théorie des graphes : arbre couvrant minimal, plus court chemin, flot maximum... Utilisation de la programmation orientée objet et de templates."
    url: https://github.com/fortierq/SGL
  - image_path: assets/images/portfolio/hirsch.png
    title: "Conjecture de Hirsch"
    excerpt: Recherche d'un contre-exemple à la conjecture de Hirsch en utilisant le SMT-solver Z3 en C++. La conjecture de Hirsch affirme que le diamètre d'un polytope de dimension $$d$$ avec $$n$$ faces est inférieur à $$n - d$$.
    url: https://github.com/fortierq/Prismatoid
---

<center>
<video width="640" height="480" controls autoplay loop>
  <source id="anim" type="video/mp4" src="">
  Your browser does not support the video tag.
</video>
</center>

{% include feature_row_anim %}

<!-- {% include feature_row id="feature_row2" %} -->
