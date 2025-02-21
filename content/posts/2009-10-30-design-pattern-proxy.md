---
title: 'Design Pattern : Proxy'
author: ogirardot
type: post
date: 2009-10-30T05:30:36+00:00

original_post_id:
  - 292
categories:
  - Java
tags:
  - Bonnes pratiques
  - coding standards
  - Design patterns
  - Proxy

---
<!--more-->
Continuons un peu notre tour d'horizon avec le design pattern Proxy. Un proxy est, de par la nature du mot, souvent une interface entre plusieurs composants, servant de pont entre ceux-ci.

Avec un Proxy, on peut contrôler des objets, tout en manipulant des &#8220;remplaçants&#8221;. Un exemple souvent pris dans la littérature est celui d'une image à charger. Si l'on cherche à peindre l'image directement (surtout si c'est une grosse image), on risque de faire ramer notre application en monopolisant son processus principal, il est donc préférable de mettre un petit &#8220;Loading image ...&#8221; et faire tourner le chargement en tâche de fond.

Un exemple plus explicite consiste en Java-RMI (Remote Method Invocation) à utiliser un &#8220;représentant&#8221; local pour manipuler un objet distant, la technologie se chargeant alors de la communication et de la transmission des exceptions.

Pour ceux qui aimerait plus d'information et une implémentation explicite de ce pattern, voilà un petit lien avec l'exemple des images (ImageProxy) implémenté : <a href="http://perfectjpattern.sourceforge.net/dp-proxy.html" target="_blank">http://perfectjpattern.sourceforge.net/</a> et un mini-framework pour mettre en application ce design.