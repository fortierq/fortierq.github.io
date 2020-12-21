---
title: "Teaching"
toc: true
toc_sticky: true
permalink: /teaching/
date: now
courses:
  - name: Option informatique en MP*
    url: /assets/teaching/MP/
    chapters:
        - name: Structures de données
          lectures: 
            - name: "Cours 1: pile, file, arbre binaire de recherche, dictionnaire" 
              url: structures.pdf
            - name: "Cours 2: file de priorité et tas"
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
            - name: "Cours 1: définitions"
              url: graphes1.pdf
            - name: "Cours 2: représentation et parcours"
              url: graphes2.pdf
            - name: "Cours 3: plus courts chemins"
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
            - name: "Cours 1: langage rationnel"
              url: langage.pdf
            - name: "Cours 2: automate régulier"
              url: automate.pdf
            - name: "Cours 3: équivalence"
              url: equiv.pdf
          exos:
            - name: TD1
              url: td_langage_cor.pdf 
            - name: TD2
              url: td_automate.pdf
            - name: TD3
              url: td_equiv_cor.pdf

        - name: Logique
          lectures: 
            - name: "Cours: formule, table de vérité, satisfiabilité"
              url: logique.pdf
          exos:
            - name: TD
              url: td_logique.pdf 
            - name: DS
              url: DS_logique.pdf
            - name: DM
              url: DM_logique.pdf
            
  - name: Informatique commune en PSI
---

{% for course in page.courses %}
## {{ course.name }}
{% for chapter in course.chapters %}
### {{ chapter.name }}  
{% for lecture in chapter.lectures %} 
- [{{ lecture.name }}]({{ course.url | append: lecture.url }})
{% endfor %}  
- Exercices: {% for exo in chapter.exos %} [{{ exo.name }}]({{ course.url | append: exo.url }}) {% endfor %}
{% endfor %}
{% endfor %}