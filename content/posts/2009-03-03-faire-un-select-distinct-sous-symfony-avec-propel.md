---
title: Faire un “Select DISTINCT …” sous Symfony avec Propel
author: ogirardot
type: post
date: 2009-03-03T07:10:00+00:00

blogger_blog:
  - readtfb.blogspot.com
blogger_author:
  - ssaboumhttp://www.blogger.com/profile/15320526353188755053noreply@blogger.com
blogger_permalink:
  - /2009/03/faire-un-select-distinct-sous-symfony.html
original_post_id:
  - 12
categories:
  - Uncategorized

---
<!--more-->
Il n'est jamais inutile surtout quand on possède un grand jeu de données de pouvoir faire un SELECT DISTINCT dans ses requètes, pour le faire Propel possède pour les Criteria une méthode <span style="font-style:italic;">SetDistinct</span>(), mais le problème c'est que cette méthode, dans le cas général, va générer le code SQL suivant :

<pre><br />SELECT DISTINCT individu_id, individu_nom, individu_prenom, .....<br /></pre>

et qu'a posteriori la requète sera hydratée pour récupérer les Objets &#8220;Individu&#8221; qui seront bel et bien unique, vis-à-vis de tous leurs attributs, mais pas pour une colonne précise comme le nom par exemple.

Ce petit article va vous donner un moyen et une explication pour passer au delà de cette limitation qui va avec le fonctionnement normal de Propel.

Tout d'abord nous allons partir du principe que vous souhaitez récupérer dans la base de donnée tous les noms Uniques au sein de la table individu. Donc on commence a construire le Criteria :

<pre><br />$c = new Criteria();<br />$c-&gt;addSelectColumns(IndividuPeer::INDIVIDU_NOM);<br />$c-&gt;SetDistinct();<br /></pre>

Seulement si après vous essayer de faire un <span style="font-style:italic;">doSelect</span>, Propel ressortira une erreur d'offset, car lors de la tentative d'hydratation, il cherchera des colonnes qui n'existent pas dans notre Select, car la requète ainsi formée est bel et bien :

<pre><br />SELECT DISTINCT individu.individu_nom FROM individu;<br /></pre>

Pour préciser à Propel que l'on ne veut que le résultat de la requête il ne faut donc pas faire un doSelect, mais ceci :

<pre><br />$individu = IndividuPeer::doSelectRS($c);<br />// ou dans symfony 1.2 :<br />$individu = IndividuPeer::doSelectStmt($c);<br />//on récupère alors les données dans la matrice associative $individu par :<br />echo $individu['individu_nom'];<br /></pre>

voilà, vous savez tout, et ça vous évitera (pour cette fois) de finir par faire vos requêtes à la main, en bypassant l'ORM. 🙂