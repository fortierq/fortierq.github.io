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
---

## Thèse : << Aspects of connectivity with matroid constraints in graphs >>
[**Manuscrit**](https://tel.archives-ouvertes.fr/tel-01838231)  [**Soutenance**](/assets/soutenance.pdf)  

*Abstract*:  
The notion of connectivity is fundamental in graph theory. We study in details a recent development in this field, with the addition of matroid constraints. We show that some important results extends to this new theory. In particular, we will focus on packing of paths and arborescences in highly connected graphs with matroid constraints.


## Publications
- Rapporteur pour *Journal of Graph Theory*
{% for pub in page.publications %}
- [{{ pub.name }}]({{ pub.url }})  
*{{ pub.journal }}*, {{ pub.date }}
{% endfor %}
