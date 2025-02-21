---
title: Args4j ou de l’utilisation de Java pour faire des Batchs
author: ogirardot
type: post
date: 2009-06-02T17:09:00+00:00

blogger_blog:
  - readtfb.blogspot.com
blogger_author:
  - ssaboumhttp://www.blogger.com/profile/15320526353188755053noreply@blogger.com
blogger_permalink:
  - /2009/06/args4j-ou-de-lutilisation-de-java-pour.html
original_post_id:
  - 27
categories:
  - Uncategorized

---
<!--more-->
Parce que ça fait longtemps que je n'ai pas parlé d'informatique, je vais présenter cette petite librairie qui permet de faire les choses proprement quand on conçoit une application Java utilisable en ligne de commande.

En effet la plupart du temps quand on devait créer une telle application, on créait une méthode un peu comme cela :

<pre><br />public static void main(String[] args){<br />Class instance = new Class();<br /><br />// récupération du nom de l'individu :<br />instance.nom = args[0];<br /><br />// récupération de l'age de l'individu :<br />instance.age = Integer.valueOf(args[1]);<br /><br />// récupération du boolean vie/mort :<br />instance.isMort = Boolean.valueOf(args[2]);<br /><br />}<br /></pre>

En supposant bien sûr aucune vérification, aucune robustesse, et surtout un code on ne peut plus clair...  
[Args4j][1] permet de mettre un peu plus de formalisme derrière tout ça, ainsi on va pouvoir rapprocher l'utilisation d'un programme java de celui de n'importe quel script sh ou python utilisable sous unix (avec des paramètres de la forme -name &#8220;Olivier&#8221;).  
La seule contrainte est de définir les paramètres que va devoir traiter [args4j][1] en tant que variables membres de la classe, ce qui reste une toute petite contrainte face à l'économie de prise de tête que représente l'outil.  
Ainsi pour reprendre mon exemple avec un individu, on va reformater les élèments du programme sous cette forme :

<pre><br />import org.kohsuke.args4j.Argument;<br />import org.kohsuke.args4j.CmdLineException;<br />import org.kohsuke.args4j.CmdLineParser;<br />import org.kohsuke.args4j.Option;<br />import org.kohsuke.args4j.spi.IntOptionHandler;<br />import org.kohsuke.args4j.spi.BooleanOptionHandler;<br />import java.io.IOException;<br /><br />public class Individu {<br /><br />@Option(name = "-n",aliases = {"--name"},usage="Sets a name for the person", required = true)<br />private String nom;<br /><br />@Option(name = "-y", aliases = {"--years"},handler= IntOptionHandler.class, usage ="Define the person's age" )<br />private Integer age;<br /><br />@Option(name="--isdead", handler = BooleanOptionHandler.class, usage = "Define if the person is dead")<br />private boolean isMort;<br /><br /><br />public void doMain(String[] args) throws IOException {<br />CmdLineParser parser = new CmdLineParser(this);<br /><br />try {<br /><br />// parse the arguments.<br />parser.parseArgument(args);  <br /><br />} catch( CmdLineException e ) {<br />System.err.println(e.getMessage());<br /><br />// on va réafficher les possibilités d'arguments du programme<br /><br />parser.printUsage(System.err);<br />return;<br />}<br />}<br /><br />public static void main(String[] args) throws IOException{<br />Individu jean = new Individu();<br />jean.doMain(args);<br />}<br />}<br /></pre>

Grâce à ce simple code et le tag des variables dont on avait besoin, ainsi que la définition de handler vérifiant que les paramètres donnés sont bel et bien du type attendu ou tout simplement qu'ils sont présent.  
Ainsi le code ci-dessus une fois utilisé en ligne de commande donne le résultat suivant :

[<img decoding="async" style="margin:0 auto 10px;display:block;text-align:center;cursor:pointer;width:400px;height:88px;" src="http://2.bp.blogspot.com/_qTrjuYjK_hI/SiVxWpA4PJI/AAAAAAAAAC4/9WxiyZjZUFI/s400/Image+3.png" alt="" border="0" />][2]  
Il est encore plus intéressant de pouvoir définir ses propres handler, et de pouvoir appliquer cette notion d'Options à des méthodes qui se chargeront de remplir plusieurs paramètres à la fois. Enfin tout ça vous pourrez le découvrir si vous vous intéressez un peu plus à la librairie que l'on doit à [Kohsuke Kawaguchi][3].

 [1]: http://args4j.dev.java.net/
 [2]: http://2.bp.blogspot.com/_qTrjuYjK_hI/SiVxWpA4PJI/AAAAAAAAAC4/9WxiyZjZUFI/s1600-h/Image+3.png
 [3]: http://weblogs.java.net/blog/kohsuke/