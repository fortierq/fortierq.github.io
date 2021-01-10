---
title: "Installer Ubuntu sur une clé USB"
tags:
  - linux
  - hardware
toc: true
toc_sticky: true
header:
  teaser: /assets/images/ubuntu.svg
  og_image: /assets/images/ubuntu.svg
---

Dans ce court article, je présente l'installation (*full install*) de Ubuntu sur clé USB. Il y a de bonnes chances que le procédé ci-dessous fonctionne pour la plupart des autres distributions Linux, même si je n'ai pas testé.

**Note** : Ce tutoriel est en construction.  
{: .notice--danger}


# Pourquoi?

L'intérêt est de pouvoir utiliser son OS et système de fichiers sur n'importe quel ordinateur pouvant démarrer sur la clé USB, sans installer quoi que ce soit sur cet ordinateur.  
Il existe plusieurs possibilités:
- Utiliser un [live USB](https://doc.ubuntu-fr.org/live_usb) simple, à partir d'une image .iso. C'est utilisé généralement pour installer Ubuntu, mais il est aussi possible d'essayer/utiliser Ubuntu sans rien installer. Les données sont non-persistantes : une fois l'ordinateur éteint, tous les fichiers et logiciels ajoutés sur la clé USB sont supprimées.
- Utiliser un live USB persistant avec par exemple [mkusb](https://doc.ubuntu-fr.org/mkusb).  La clé USB est partitionné en plusieurs disques:
  - disque contenant l'image .iso Ubuntu 
  - disque `writable` où les données sont sauvegardées et conservées après un redémarrage
  - disque de démarrage
Cependant, il y a plusieurs inconvéniants:
  - aucun mot de passe
  - mise à jour du noyau difficile/impossible
  - démarrage plus lent
- Installer Ubuntu directement sur la clé USB, très similaire à une installation classique sur disque dur.

