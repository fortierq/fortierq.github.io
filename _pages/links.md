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
          author: Guido van Rossum
          url: http://neopythonic.blogspot.com/ 
        - name: "Python Developer’s Guide"
          url: https://devguide.python.org/
        - name: "Curious Efficiency blog"
          author: Nick Coghlan
          url: https://www.curiousefficiency.org/
  - name: "Jupyter" 
    items:
        - name: Using jupyter notebooks with a virtual environment
          author: Angelo Basile
          url: https://anbasile.github.io/posts/2017-06-25-jupyter-venv/
  - name: "Git"
    items:
        - name: Merge vs rebase
          author: Mislav Marohnić
          url: https://mislav.net/2013/02/merge-vs-rebase/
  - name: "Machine Learning"
    items:
        - name: "Optimisation, apprentissage statistique"
          author: Francis Bach
          loc: ENS et Paris-Sud
          url: https://www.di.ens.fr/~fbach/#courses
        - name: "Deep Learning"
          author: Yann LeCun, Alfredo Canziani
          loc: NYU
          url: https://atcold.github.io/pytorch-Deep-Learning/
        - name: "ML/DL with fastai and Pytorch"
          url: https://www.fast.ai
          author: Jeremy Howard, Sylvain Gugger
        - name: "Computational and Inferential Thinking: The Foundations of Data Science"
          author: Ani Adhikari, John DeNero
          loc: NYU
          url: https://inferentialthinking.com/chapters/intro.html
        - name: "Wikistat"
          author: Philippe Besse, Brendan Guillouet
          loc: INSA Toulouse
          url: http://wikistat.fr/
        - name: "Computational and Inferential Thinking: The Foundations of Data Science"
          author: Ani Adhikari, John DeNero
          loc: NYU
          url: https://inferentialthinking.com/chapters/intro.html
  - name: "Informatique graphique"
    items:
        - name: Ray Tracing in One Weekend
          author: Peter Shirley
          loc: Nvidia
          url: https://raytracing.github.io/books/RayTracingInOneWeekend.html
        - name: Accelerated Ray Tracing in One Weekend in CUDA
          author: Roger Allen
          loc: Nvidia
          url: https://developer.nvidia.com/blog/accelerated-ray-tracing-cuda/
---

{% for theme in page.themes %}

## {{theme.name}}

{% for item in theme.items %}

- [{{item.name}}]({{item.url}}){%if item.author %} ({{item.author}}{% if item.loc %} - {{item.loc}}{% endif %}){% endif %}
{% endfor %}
{% endfor %}
