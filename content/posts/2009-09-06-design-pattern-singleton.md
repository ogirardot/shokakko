---
title: 'Design Pattern : Singleton'
author: ogirardot
type: post
date: 2009-09-06T08:29:22+00:00

original_post_id:
  - 203
categories:
  - Uncategorized

---
<!--more-->
Dans la série des questions d'entretien, et des choses utiles, je vais passez en revue les design pattern les plus courants et les plus utiles.

Alors commençons tout d'abord par le **Singleton** :

Ce design pattern, ou Patron de conception, permet de créer une et une seule instance d'une classe et de renvoyer, quand une classe tiers demande une nouvelle instance, toujours la même. Il permet à plusieurs programme de tous partager la même instance d'une classe.

Le meilleur exemple d'utilisation restant la connection à une base de donnée. Car au sein d'une application on ne va jamais ouvrir plusieurs connection, par contre chaque programme qui exécute des requêtes aura besoin de récupérer le connection. D'où la solution suivante souvent utilisée d'un **Singleton** _DbConnectionManager_ :

<pre>// récupérer la connection :
DbConnectionManager.getInstance().getConnection();</pre>

L'implémentation d'un singleton est simple, il suffit de définir le constructeur comme privé, et de conserver une variable static représentant l'instance à renvoyer de notre classe :

<pre>public class Singleton {

  private static Singleton instance;

  private Singleton() { /* Do nothing... */ }

  public static Singleton getInstance() {
    if (instance == null)
      instance = new Singleton();
    return instance;
  }
}</pre>

Voilà comment créer un singleton, et factoriser les accès à une ressource. Quand le singleton est utilisé dans un contexte multi-thread, il est nécessaire que la méthode _getInstance_ soit **synchronized** (Verrou d'exclusion mutuelle) pour que lors de l'appel simultané à cette méthode de deux processus léger, l'un créer une nouvelle instance (si inexistante) et l'autre se récupère la première et unique instance.