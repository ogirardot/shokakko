---
title: 'Précis de SQL : utilisation de “Minus”'
author: ogirardot
type: post
date: 2009-09-05T12:26:21+00:00

original_post_id:
  - 180
categories:
  - Uncategorized

---
<!--more-->
S'il est une chose qui arrive souvent quand on cherche à optimiser ou reprendre une application, c'est de tomber sur LA requête SQL...  
Celle qui a du prendre 3 jours à être faite par une personne qui, très probablement, ne pensait pas ou ne voulait pas que quelqu'un, autre que lui (et encore...), la comprenne.

Alors si parfois quelqu'un ou quelques commentaires peuvent permettre de voir où le code autour voulait &#8220;fonctionnellement&#8221; en venir, le plus souvent la requête reste intouchée et vierge de tous commentaire autre que :

<pre>/**
* FIXME : dirty and un-comprehensible... but for now this
* should do the trick...
* TODO : Embaucher un stagiaire pour en faire une analyse détaillée
**/</pre>

Mais heureusement que je suis là (et stagiaire...) et que je vais enfin prouver deux choses,

  * la première : Mysql n'est pas une base de donnée... ;
  * la deuxième : ce blog est utile (mais si !!) ;

Donc souvent pour faire une refactoring de ce genre de requête un être humain censé possédant une légère maitrise de ce que devrait faire la requête, va tout simplement tenter de la ré-écrire, et quand il retrouvera un résultat... quasi-similaire, la seule peur qu'il aura sera de savoir si les deux requêtes font bel et bien le même travail...

Et c'est là que rentre en jeu la clause **MINUS**, statement standard de SQL92, supporté par Oracle et PostGreSQL, mais pas par Mysql...(first fact). Qui permet de réaliser une simple vérification entre deux requêtes _query1_ et _query2_, en utilisant votre requêteur préféré (e.g. <a title="Toad" href="http://www.toadsoft.com/" target="_blank">Toad</a>) et en exécutant:

<pre>query1
minus
query2</pre>

Alors vous allez obtenir les tuples qui ressortent avec une requête mais pas l'autre. Cette opération a un sens, pour être vraiment sûr il faut donc intervertir les deux requêtes et répéter le procéssus.

Enjoy