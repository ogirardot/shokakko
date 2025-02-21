---
title: Lire un Dvd sous Ubuntu – Cas d’école de piratage
author: ogirardot
type: post
date: 2009-05-03T18:31:00+00:00

blogger_blog:
  - readtfb.blogspot.com
blogger_author:
  - ssaboumhttp://www.blogger.com/profile/15320526353188755053noreply@blogger.com
blogger_permalink:
  - /2009/05/lire-un-dvd-sous-ubuntu-cas-decole-de.html
original_post_id:
  - 23
categories:
  - Uncategorized

---
<!--more-->
Simple problème, comment pouvoir lire un DVD sous linux Ubuntu ?  
On ne parle pas de piratage, pas de ripper ce DVD, pas de lui extraire les sous-titres ou autres, juste de le lire en toute légalité. Alors comme d'habitude la meilleure solution reste VLC, seulement si vous essayer de le lire avec un système contenant VLC - Ubuntu Jaunty, il finit par un <span style="font-style:italic;">segmentation fault</span>, dont la trace montre qu'il lui manque une librairie.

Ainsi pour finir par lire le dvd correctement il faut avoir déjà présent comme librairie <span style="font-style:italic;">libdvdread4</span>, mais cela ne suffit pas, il faut aussi lancer la commande suivante :

<pre>sudo /usr/share/doc/libdvdread4/install-css.sh</pre>

Mais pourquoi être obligé d'utiliser cette commande et quels en sont les tenants et aboutissants ?

Comme tout le monde le sait la plupart des DVD utilisent à l'heure actuelle des systèmes anti-copies, dont l'acronyme actuel est CSS - Content Scrambling System. Seulement ces systèmes n'empêchent en théorie pas de lire les DVD, mais les lecteurs doivent alors récupérer auprès de la [DVD Copy Control Association,][1] les jeux de clés, pour pouvoir décrypter les informations. Car en somme cette association vend son système aux créateurs et distributeurs de DVD, mais elle ne propose pas de possibilités d'utilisation par des systèmes d'exploitation libre.

Néanmoins, ce système de cryptage a été cryptanalysé et cassé en 1999, et VideoLan a conçu la librairie libdvdcss pour permettre de lire les DVD avec VLC.

Mais aux États-Unis la loi DMCA et en France la loi DADVSI rendaient potentiellement illégale l'utilisation et/ou la distribution de la librairie libdvdcss.

Heureusement en France, l'arrêt du Conseil d'État du 16 juillet 2008 lève l'ambigüité, en confirmant notamment que l'utilisation d'un logiciel libre, interopérant avec une mesure technique à l'aide d'informations obtenues par décompilation des éléments logiciels de cette dernière, n'a rien d'illicite au regard de la loi DADVSI et de ce décret.

Le fait est que même si maintenant au regard de la loi française ce n'est plus illégale de pouvoir lire ses DVD achetés légalement, au regard de la loi américaine, la chose est toujours ambigüe et Canonical ne souhaitant pas se faire attaquer, elle laisse le soin à ses utilisateurs de faire le choix. On en revient donc au Flow Chart d'xkcd sur le sujet :

[<img decoding="async" style="margin:0 auto 10px;display:block;text-align:center;cursor:pointer;width:498px;height:469px;" src="http://imgs.xkcd.com/comics/steal_this_comic.png" border="0" alt="" />][2]  
<span style="font-style:italic;">Cet article n'est pas du tout, bien sûr, une incitation au téléchargement de fichiers piratés, ou à une violation de la propriété privée.</span>

 [1]: http://en.wikipedia.org/wiki/DVD_Copy_Control_Association "DVD Copy Control Association"
 [2]: http://imgs.xkcd.com/comics/steal_this_comic.png