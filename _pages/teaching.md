---
title: "Teaching"
permalink: /teaching/
date: now
courses:
  - name: Option informatique en MP*
    url: /assets/teaching/MP/
    lectures:
        - name: Structures de données 1
          url: structures.pdf
          exos:
            - name: TD1
              url: td_structures.pdf 
            - name: TD2
              url: td_structures2.pdf 
            - name: DM
              url: DM_structures.pdf
  - name: Informatique commune en PSI
---

{% for course in page.courses %}
## {{ course.name }}
{% for lecture in course.lectures %}
- [**{{ lecture.name }}**]({{ course.url | append: lecture.url }})  
Exercices: {% for exo in lecture.exos %} [{{ exo.name }}]({{ course.url | append: exo.url }}) {% endfor %}
{% endfor %}
{% endfor %}