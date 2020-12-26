---
title: "Outils pour le développement Python"
tags:
  - python
  - programmation
toc: true
toc_label: "Outils pour le développement Python"
toc_sticky: true
---

Alors que Python poursuit sa [croissance exceptionnelle](https://www.tiobe.com/tiobe-index), la multiplication des outils à disposition du développeur Python peut être déconcertante. Essayons d'y voir plus clair.

**Note** : Tout ce qui a été écrit ici a été testé sous Ubuntu/Debian, mais reste valable sous Windows, avec un peu plus d'efforts. Les versions récentes de Windows permettent en outre d'utiliser Linux via [WSL](https://docs.microsoft.com/fr-fr/windows/wsl/install-win10).  
{: .notice--info}

## Gérer les versions Python : pyenv

Commençons par le commencement: installer Python. Tous les systèmes Linux récents ont Python nativement mais changer de version à partir des sources peut être fastidieux. [pyenv](https://github.com/pyenv/pyenv) simplifie l'installation et l'utilisation de différentes versions de Python.  
Pour installer pyenv, le plus simple est de passer par ce [script](https://github.com/pyenv/pyenv-installer):

~~~ console
curl https://pyenv.run | bash
~~~

Ceci télécharge puis exécute [https://pyenv.run](https://pyenv.run), qui clone le dépôt [github.com:pyenv/pyenv](https://github.com/pyenv/pyenv) dans `~/.pyenv` et ajoute `pyenv` dans `~/.bashrc`.  
pyenv utilise un hook pour choisir la bonne version de Python à utiliser.  
Pour voir les versions de Python détectées:
~~~
pyenv global
~~~
**Note** : Sous Ubuntu, il ya une commande `python3` mais pas `python`, ce qui empêche pyenv de le détecter. Une solution est de créer un lien symbolique: `ln -s /usr/bin/python3 /usr/bin/python`.
{:  .notice--info}

Installons les éventuelles dépendances manquantes de Python: 
~~~
sudo apt-get update; sudo apt-get install --no-install-recommends make build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev
~~~

Ajoutons la version 3.9.1 de Python:
~~~
pyenv install 3.9.1
~~~
{:  .notice--info}

Il est possible de spécifier une version de Python au niveau global ou local:
~~~
pyenv global 3.9.1 # utiliser python3.9.1 globalement
pyenv local 3.9.1 # utiliser python3.9.1 localement (répertoire en cours)
~~~
{:  .notice--info}

`python`, `python3` ou `python3.9` permet alors d'utiliser notre version 3.9.1 fraîchement installée.

## IDE : PyCharm vs Visual Code 

![image-right](/assets/img/ide_python.png){: .align-right} 

PyCharm et Visual Code sont, de loin, les environnements de développement privilégiés par les développeurs Python.[^1]
Alors que PyCharm est spécifique au langage Python, Visual Code est un EDI "tout en un" qui est très utilisé aussi en développement web, par exemple. 
À noter que PyCharm est gratuit dans sa version *Community* mais payante et propriétaire en version *Professional*. Au contraire, Visual Code est entièrement gratuit et open source. J'ai personnellement adopté Visual Code pour sa légereté et ses nombreux plugins.

Voici quelques commandes Visual Code que j'ai trouvé particulièrement utiles:
- **Ctrl + Maj + P / F1**: ouvrir la liste des commandes  
  Certaines commandes n'ont pas de raccourci mais sont très pratiques: "Transform to Lower Case" ou "Sort Imports", par exemple  
- **Ctrl + P**: naviguer parmi les fichiers/fonctions
- **Ctrl + F5**: exécuter un fichier
- **F5**: débugger un fichier
- **Ctrl + C / Ctrl + V / Ctrl + X** (sans sélection): copier/coller/couper la ligne en cours
- **Alt + Up/Down**: déplacer une ligne vers le haut/bas
- **Alt + Clic**: selection multi-curseurs
- **Ctrl + Maj + L**: selectionner tous mots identiques

## Dépendances et packaging : Poetry vs Pipenv

## Conventions de nommage : snake case

## Docstrings : numpy

## Linting 

## Formatting

## AI complétion : tabnine

## Tests: pytest

## Code coverage

## Références

[^1]: [Python Developers Survey 2019](https://www.jetbrains.com/lp/python-developers-survey-2019), mené par la Python Software Foundation et... Jetbrains, éditeur de PyCharm.
- https://cjolowicz.github.io/posts/hypermodern-python-01-setup/