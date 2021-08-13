---
permalink: /links/
title: "Liens"
toc: true
toc_label: "Liens"
toc_sticky: true
themes:
  - name: "Python"
    items:
        - name: "Python Packaging User Guide"
          author: "Python Packaging Authority"
          url: https://packaging.python.org/
  - name: "Machine learning & Data science"
    items:
        - name: User guide
          author: scikit-learn
          url: https://scikit-learn.org/stable/user_guide.html
        - name: Site de Xavier Dupré
          url: http://www.xavierdupre.fr/
        - name: Bayesian Methods for Hackers
          author: Cameron Davidson-Pilon
          url: http://camdavidsonpilon.github.io/Probabilistic-Programming-and-Bayesian-Methods-for-Hackers/#contents
        - name: Exercices pratiques
          author: Marie Chavent
          loc: Bordeaux
          url: http://www.math.u-bordeaux.fr/~mchave100p/teaching/
        - name: "Cours science des données"
          author: François Husson
          loc: Agrocampus Rennes
          url: https://husson.github.io/teaching.html
        - name: "Machine Learning for Beginners - A Curriculum"
          author: Microsoft
          url: https://github.com/microsoft/ML-For-Beginners
        - name: "Optimisation, apprentissage statistique"
          author: Francis Bach
          loc: ENS et Paris-Sud
          url: https://www.di.ens.fr/~fbach/#courses
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
  - name: Deep learning
    items:
        - name: Dive into Deep Learning
          author: Aston Zhang, Zack C. Lipton, Mu Li, Alex J. Smola
          url: http://d2l.ai/index.html
        - name: "Deep Learning"
          author: Yann LeCun, Alfredo Canziani
          loc: NYU
          url: https://atcold.github.io/pytorch-Deep-Learning/
        - name: Hugging Face Course (NLP, transformers)
          author: Carrigan, Debut, Gugger
          url: https://huggingface.co/course/chapter1
        - name: Deep Learning Do It Yourself!
          author: Marc Lelarge, Jill-Jênn Vie, Andrei Bursuc
          loc: ENS, X
          url: https://dataflowr.github.io/website/
  - name: Optimisation
    items:
        - name: Optimisation pour l'économie
          author: Laurent Guillopé
          loc: Université de Nantes
          url: https://www.math.sciences.univ-nantes.fr/~guillope/l3-osc/osc.pdf
        - name: Optimization for Data Science
          author: Yaoliang Yu
          loc: University of Waterloo
          url: https://cs.uwaterloo.ca/~y328yu/mycourses/794-2020/lecture.html
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
  - name: Javascript
    items:
        - name: MDN
          author: Mozilla
          url: https://developer.mozilla.org/en-US/ 
        - name: javascript.info
          url: https://javascript.info/
---

{% for theme in page.themes %}

## {{theme.name}}

{% for item in theme.items %}

- [{{item.name}}]({{item.url}}){%if item.author %} ({{item.author}}{% if item.loc %} - {{item.loc}}{% endif %}){% endif %}
{% endfor %}
{% endfor %}
