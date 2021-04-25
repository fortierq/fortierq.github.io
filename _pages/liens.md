---
permalink: /links/
title: "Liens"
toc: true
toc_label: "Liens"
toc_sticky: true
themes:
  - name: "Python"
    items:
        - name: "Documentation officielle"
          url: https://docs.python.org/3/
        - name: "Neopythonic blog"
          autor: Guido van Rossum
          url: http://neopythonic.blogspot.com/ 
        - name: "Python Developer’s Guide"
          url: https://devguide.python.org/
          
  - name: "Machine Learning"
    items:
        - name: "Optimisation, apprentissage statistique"
          autor: Francis Bach
          loc: ENS et Paris-Sud
          url: https://www.di.ens.fr/~fbach/#courses
        - name: "Deep Learning"
          autor: Yann LeCun, Alfredo Canziani
          loc: NYU
          url: https://atcold.github.io/pytorch-Deep-Learning/
        - name: "ML/DL with fastai and Pytorch"
          url: https://www.fast.ai
          autor: Jeremy Howard, Sylvain Gugger
        - name: "Computational and Inferential Thinking: The Foundations of Data Science"
          autor: Ani Adhikari, John DeNero
          loc: NYU
          url: https://inferentialthinking.com/chapters/intro.html
        - name: "Wikistat"
          autor: Philippe Besse, Brendan Guillouet
          loc: INSA Toulouse
          url: http://wikistat.fr/
        - name: "Computational and Inferential Thinking: The Foundations of Data Science"
          autor: Ani Adhikari, John DeNero
          loc: NYU
          url: https://inferentialthinking.com/chapters/intro.html
---

{% for theme in page.themes %}

## {{theme.name}}

{% for item in theme.items %}

- [{{item.name}}]({{item.url}}) ({{item.autor}}{% if item.loc %} - {{item.loc}}{% endif %})
{% endfor %}
{% endfor %}
