---
title: Evitons Javascript
author: ogirardot
type: post
date: 2010-09-08T10:00:47+00:00

original_post_id:
  - 491
categories:
  - Uncategorized
tags:
  - gwt
  - javascript
  - vaadin

---
<!--more-->
<p style="text-align:justify;">
  S'il y a bien une chose qui semble évidente si on regarde le monde du web actuel se dessiner, c'est que (presque) tous les frameworks actuels tentent d'<strong>éliminer complètement Javascript </strong>des préoccupations des développeurs. Les initiatives ne manquent pas et les systèmes complexes non plus :
</p>

<ul style="text-align:justify;">
  <li>
    GWT - édité par Google, permet de générer du Javascript compatible avec tous les navigateurs à partir de code Java ;
  </li>
  <li>
    Vaadin - sur-couche à GWT simplifie encore plus le processus en incorporant plus de widgets de base ;
  </li>
  <li>
    SproutCore - framework Ruby génère du HTML et du Javascript pour créer une RIA complète ;
  </li>
  <li>
    Silverlight ;
  </li>
  <li>
    Flex ;
  </li>
</ul>

<p style="text-align:justify;">
  J'en passe un certains nombre sous silence et je n'évoque même pas l'excitation de certains sur HTML5 considèrant cette initiative comme le messie. Face à tout ça, il est difficile, en tant que développeur Java et Python, de dire aujourd'hui <strong><span style="text-decoration:underline;">j'ai choisi Javascript !</span></strong>
</p>

<p style="text-align:justify;">
  Et ceci pour plusieurs raisons :
</p>

<ol style="text-align:justify;">
  <li>
    Au lieu d'aller à contre-courant en créant un système plus compliqué que le problème qu'il est censé résoudre (à savoir Javascript), je préfère mettre les mains dans le cambouis et m'en sortir en ayant appris quelque chose ;
  </li>
  <li>
    Les performances sont toujours dégradés quand on créé une sur-couche à un système low-level (sinon pourquoi ferions-nous encore du C ?) ;
  </li>
  <li>
    Les librairies Javascripts actuelles : jQuery, Dojo, Scriptaculous, MooTools, simplifient tellement les développements que pour beaucoup de fonctionnalités la compatibilité cross-navigateur est assurée ;
  </li>
  <li>
    Les environnements de déboggage avec Firebug et l'inspection dans Google Chrome permettent de ne plus se débrouiller avec des <em>alert() ;</em>
  </li>
  <li>
    De plus, même si pour certaines fonctionnalités j'utilise des méthodes HTML5, elles ne seront pas fiables avant quelques années.
  </li>
</ol>

<p style="text-align:justify;">
  Je ne compte pas le fait que mes applications actuelles utilisent pour beaucoup l'API Google Maps qui, bien qu'existantes pour GWT et Vaadin, reste beaucoup plus riche, performante et cohérente en Javascript.
</p>

<p style="text-align:justify;">
  Il y a un historique derrière cette envie de laisser Javascript dans sa tombe.
</p>

<p style="text-align:justify;">
  Javascript c'était le web <em>kikoo-lol </em> avec <em>mister infonie</em> au temps sacré de club-internet...
</p>

<p style="text-align:justify;">
  Seulement si le seul pouvoir de javascript résidait dans le fait de pouvoir poursuivre ta souris avec une bannière étoilée, je pense que non-seulement cet age là est mort, mais surtout d'autres utilisations sont apparues et sont maintenant adoptées par le grand-public.
</p>

<p style="text-align:justify;">
  Se mettre en marge de ça, c'est détruire un pan énorme de l'Expérience Utilisateur de ses visiteurs...
</p>

<p style="text-align:justify;">
  A vous de voir si ça en vaut la peine,
</p>

<p style="text-align:justify;">
  <em>Vale</em>
</p>