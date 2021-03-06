---
toc: true
toc_label: "Enseignement"
toc_sticky: true
title: "Enseignement"
permalink: /teaching/
courses:
  - name: Option informatique en MP/MP*
    url: /assets/teaching/MP/
    chapters:
        - name: Structures de données
          lectures: 
            - name: "pile, file, arbre binaire de recherche, dictionnaire" 
              url: structures.pdf
            - name: "file de priorité et tas"
              url: structures2.pdf
          exos:
            - name: TD1
              url: td_structures.pdf 
            - name: TD2
              url: td_structures2.pdf 
            - name: DM
              url: DM_structures.pdf
            - name: DS
              url: DS_structures.pdf
        - name: Graphes
          lectures: 
            - name: "définitions"
              url: graphes1.pdf
            - name: "représentation et parcours"
              url: graphes2.pdf
            - name: "plus courts chemins"
              url: graphes3.pdf
          exos:
            - name: TD1
              url: td_graphes1_cor.pdf 
            - name: TD2
              url: td_graphes2_cor.pdf
            - name: TD3
              url: td_graphes3_cor.pdf
            - name: DS
              url: ds_graphe.pdf
        - name: Langages et automates
          lectures: 
            - name: "langage rationnel"
              url: langage.pdf
            - name: "automate régulier"
              url: automate.pdf
            - name: "équivalence"
              url: equiv.pdf
          exos:
            - name: TD1
              url: td_langage_cor.pdf 
            - name: TD2
              url: td_automate.pdf
            - name: TD3
              url: td_equiv_cor.pdf
            - name: "E3A 2019 corrigé"
              url: e3a_2019_cor.pdf
        - name: Logique
          lectures: 
            - name: "formule, table de vérité, satisfiabilité"
              url: logique.pdf
          exos:
            - name: TD
              url: td_logique.pdf 
            - name: DS
              url: DS_logique.pdf
            - name: DM
              url: DM_logique.pdf

  - name: Option informatique en MPSI
    url: /assets/teaching/MPSI/
    items:
      - name: "Cours bonus : promenade algorithmique" 
        url: promenade.pdf
      - name: "Exercice: transformée de Fourier rapide" 
        url: fft.pdf
      - name: "Exercices: diviser pour régner" 
        url: tris.pdf
      - name: "Exercice: rendu de monnaie" 
        url: rendu_monnaie.pdf

  - name: Informatique commune en PSI
    url: /assets/teaching/PSI/
    items:
      - name: "Récursivité" 
        url: recursivite.pdf
      - name: "Algorithmes de tri" 
        url: tris.pdf
      - name: "TP : démonstration d’un théorème avec l’ordinateur" 
        url: TP_153.pdf
      - name: "TP : fractales" 
        url: tp_fractale.pdf
      - name: "TP : modèle d’Ising" 
        url: tp_ising.pdf
      - name: "TP : simulation de particules" 
        url: tp_particules.pdf
      - name: "TP : simulation de planètes" 
        url: tp_planetes.pdf
      - name: "TP : probabilités 1" 
        url: tp_probas.pdf
      - name: "TP : probabilités 2" 
        url: tp_probas2.pdf
      - name: "TP : algorithme de Gram-Schmidt" 
        url: TP_produit_scalaire.pdf

  - name: Informatique commune en PCSI
    url: /assets/teaching/PCSI/
    lectures: 
      - name: "introduction à l'algorithmique" 
        url: cours1.pdf
      - name: "conditions et boucle for" 
        url: cours_if_boucles.pdf
      - name: "boucle while" 
        url: while.pdf
      - name: "fonctions" 
        url: functions-pres.pdf
      - name: "listes" 
        url: listes-pres.pdf
      - name: "preuves de programme" 
        url: preuve.pdf
      - name: "complexité" 
        url: complexite.pdf
      - name: "recherche par dichotomie dans une liste triée" 
        url: algolistes-pres.pdf
      - name: "architecture des ordinateurs" 
        url: architecture.pdf
      - name: "représentation des entiers" 
        url: representation_entiers.pdf
      - name: "représentation des flottants" 
        url: representation_flottants.pdf
      - name: "tableaux NumPy" 
        url: array.pdf
      - name: "dessins avec matplotlib" 
        url: graphisme.pdf
      - name: "méthode de dichotomie et de Newton pour résoudre f(x)=0" 
        url: newton.pdf
      - name: "intégration numérique" 
        url: integration.pdf
      - name: "Méthode d'Euler pour une équation différentielle d'ordre 1" 
        url: euler.pdf
      - name: "Méthode d'Euler pour un système d'équations différentielles" 
        url: euler2.pdf
      - name: "Manipulation des fichiers" 
        url: fichiers.pdf
      - name: "Méthode du pivot de Gauss" 
        url: gauss1.pdf
      - name: "bases sur les bases de données (SQL)" 
        url: SGBD1.pdf
      - name: "opérations sur plusieurs tables (SQL)"
        url: base_donnees2.pdf
      - name: "fonctions d’agrégation (SQL)" 
        url: base_donnees3.pdf
      - name: "SELECT imbriqués (SQL)" 
        url: base_donnees4.pdf
  # - name: "Recherche opérationnelle en L3 Miage"
  # - name: "Algorithmique et théorie des graphes en licence d'informatique"
---

Cette page regroupe une partie des ressources que j'ai utilisé pour mes cours.

{% for course in page.courses %}
## {{ course.name }}
{% if course.chapters %}
{% for chapter in course.chapters %}
- ### {{ chapter.name }}  
{% for lecture in chapter.lectures %} 
  - [Cours {{ forloop.index }}: {{ lecture.name }}]({{ course.url | append: lecture.url }})
{% endfor %}  
  - Exercices: {% for exo in chapter.exos %} [{{ exo.name }}]({{ course.url | append: exo.url }}) {% endfor %}
{% endfor %}
{% elsif course.lectures %}
{% for lecture in course.lectures %} 
  - [Cours {{ forloop.index }}: {{ lecture.name }}]({{ course.url | append: lecture.url }})
{% endfor %}  
{% else %}
{% for e in course.items %} 
  - [{{ e.name }}]({{ course.url | append: e.url }})
  {% if e.cor %} [{{ e.cor }}]({{ course.url | append: e.cor_url }}) {% endif %}
{% endfor %}  
{% endif %}
{% endfor %}
