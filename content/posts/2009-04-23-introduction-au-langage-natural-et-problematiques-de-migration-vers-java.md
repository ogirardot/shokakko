---
title: Introduction au langage Natural et problematiques de migration vers Java
author: ogirardot
type: post
date: 2009-04-23T16:42:00+00:00

blogger_blog:
  - readtfb.blogspot.com
blogger_author:
  - ssaboumhttp://www.blogger.com/profile/15320526353188755053noreply@blogger.com
blogger_permalink:
  - /2009/04/introduction-aux-langages-natural-et.html
original_post_id:
  - 19
categories:
  - Uncategorized

---
<!--more-->
Cet article porte sur le sujet de mon stage actuel qui consiste en la migration d'ancien modules en Natural. Les faits sont les suivants :  
Natural est un langage de programmation proche du Cobol créé par la société <span style="font-style:italic;">Software AG</span>, et conçu pour gérer les accès à une base de donnée hiérarchique appelé <span style="font-style:italic;">Adabas</span>. Il a de plus été adapté pour pouvoir être utilisé avec des bases de données relationnelle comme Oracle dans un contexte Unix.

Un certain nombre de fonctionnalités de Natural permettent une grande performance dans les traitements, par exemple il est possible, lors de l'exécution d'un SELECT SQL, de réaliser des traitements en parallèle en considérant le SELECT comme une boucle.  
Pareillement, il est possible de définir une vue qui va servir de variable de tampon stockant certaines colonnes de la table considérée, ceci via la commande SELECT ... INTO (INTO étant un mot clé Natural dans ce cas précis, et non plus SQL)

Voilà un exemple de code Natural permettant d'illustrer ceci, la table est nommé SQL-PERSONNEL :

<pre>DEFINE DATA LOCAL
01 PERS VIEW OF SQL-PERSONNEL
02 NAME
02 AGE
END-DEFINE

SELECT *
INTO VIEW PERS
FROM SQL-PERSONNEL
WHERE NAME LIKE 'S%'
OBTAIN NAME
IF NAME = 'SMITH'  ADD 1 TO AGE
UPDATE
END-IF

END-SELECT</pre>

Un point important est qu'en Natural toutes les variables sont en fait global, à l'échelle d'une routine, ou sous-routine.

  * Problématique de migration de Natural vers Java

Nous avons pointé du doigt certains traitements qui n'était pas commun à Java, ainsi en Java il n'est pas possible d'exécuter une requête SQL et de réaliser des traitements sur le jeu de résultat de cette requête en parallèle, ce qui pose des problème de performance car il faut alors effectuer un post-traitement sur les résultats.

De plus Natural est un langage fonctionnel qui possède une autre particularité loin de Java : les commandes ESCAPE TOP, ou ESCAPE BOTTOM permettent de réaliser des Goto et sont utilisés très souvent.  
Le plus souvent ils sont utilisés lors de boucles comme les SELECT, et s'apparentent à des <span style="font-style:italic;">continue;</span> ou des <span style="font-style:italic;">break;</span> en Java.

Le fait que Natural n'utilise que des variables globales pose des problèmes de performance avec Java et il faut donc accorder une attention particulière à ces variables pour les remplacer autant que possible par des variables locales au sein de méthodes ou par des paramètres.

De plus et pour finir le mécanisme des exceptions de Java n'existe pas en Natural, et les erreurs sont propagés grace à ces variables globales, il est donc impératif de définir les stratégies de gestion d'erreurs après analyse du programme Natural.