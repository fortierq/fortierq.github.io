---
title: "Installer Ubuntu sur une clé USB"
tags:
  - linux
  - hardware
toc: true
toc_sticky: true
header:
  teaser: /assets/images/ubuntu-install-usb/ubuntu.png
  og_image: /assets/images/ubuntu-install-usb/ubuntu.png
---

Dans ce court article, je présente l'installation (*full install*) d'Ubuntu sur clé USB. Il y a de bonnes chances que le procédé ci-dessous fonctionne pour la plupart des autres distributions Linux, même si je n'ai pas testé.


# Pourquoi?

L'intérêt est de pouvoir utiliser son OS et système de fichiers sur n'importe quel ordinateur pouvant démarrer sur la clé USB, sans installer quoi que ce soit sur cet ordinateur.  

# Comment?

Nous avons plusieurs possibilités:
- Utiliser un [live USB](https://doc.ubuntu-fr.org/live_usb) simple, à partir d'une image .iso. C'est utilisé généralement pour installer Ubuntu, mais il est aussi possible d'essayer/utiliser Ubuntu sans rien installer. Les données sont non-persistantes : une fois l'ordinateur éteint, tous les fichiers et logiciels ajoutés sur la clé USB sont supprimés.
- Utiliser un live USB persistant avec par exemple [mkusb](https://doc.ubuntu-fr.org/mkusb).  La clé USB est partitionné en plusieurs disques:
  - disque contenant l'image .iso Ubuntu 
  - disque `writable` où les données sont sauvegardées et conservées après un redémarrage
  - disque de démarrage
Cependant, il y a plusieurs inconvénients:
  - aucun mot de passe
  - mise à jour du noyau difficile/impossible
  - démarrage plus lent
- Installer Ubuntu directement sur la clé USB, très similaire à une installation classique sur disque dur.

# Préparer les clés

- Une clé USB d'installation d'Ubuntu. Celle-ci peut être créée sous Ubuntu avec le créateur de disque (installé nativement) et une [image .iso](https://ubuntu.com/download/desktop). Sous Windows, je recommande [Rufus](https://rufus.ie). L'image d'Ubuntu 20.04 pesant 2.8 Go, il vous faudra au minimum 4 Go sur votre clé USB. Une clé USB 3.0 vous permettra une installation plus rapide, mais n'est pas obligatoire.
{% include image.html url="/assets/images/ubuntu-install-usb/startup.png" caption="Création d'une clé USB bootable" %}

- Une clé USB sur laquelle vous allez installer Ubuntu. Officiellement, la recommandation pour Ubuntu 20.04 est d'avoir au moins 25 Go d'espace libre. En pratique, j'ai constaté qu'une installation minimale prend environ 6 Go. Si vous n'avez pas besoin de beaucoup d'espace, une clé de 16 Go voire 8 Go est donc envisageable. Par contre, il est clairement préférable d'utiliser une clé **USB 3.0 ou supérieure** : tout sera très lent avec de l'USB 2.0 (j'ai testé : 15 min de démarrage, 5 min d'ouverture de session...). En revanche une clé USB 3.0 vous permettra une utilisation très fluide, comparable à une installation sur SSD.

# Déconnecter les disques durs

Il est recommandé de commencer par déconnecter le(s) disque(s) dur(s) de votre ordinateur. En effet, l'installation Ubuntu pourrait à tord écraser les fichiers de démarrage sur votre ordinateur[^1], et non pas sur votre clé USB. Pour cela, allez dans les options du BIOS/UEFI (F12/F2/... au démarrage de l'ordinateur) et désactivez vos disques durs. 

**Note** : Si vous voulez vraiment avoir l'esprit tranquille, faites une copie préalable de vos fichiers importants.
{: .notice}

{% include image.html url="/assets/images/ubuntu-install-usb/dd.jpg" %} <br>

# Installer Ubuntu

Démarrez sur la clé live USB (normalement le seul moyen de démarrage possible, si vous avez désactivé vos disques durs), puis choisissez d'installer sur l'autre clé USB (normalement, à nouveau, le seul choix possible : pas de risque de se tromper), en effacant toutes les données.

{% include image.html url="/assets/images/ubuntu-install-usb/usb-install.jpg" %} <br>

Et voilà! Vous pouvez maintenant emmener Ubuntu partout avec vous. 

[^1]: Si jamais cela arrive, vous devrez réinstaller grub en utilisant par exemple [boot-repair](https://doc.ubuntu-fr.org/boot-repair) via un live USB