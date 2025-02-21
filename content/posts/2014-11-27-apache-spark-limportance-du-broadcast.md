---
title: 'Apache Spark : l’importance du broadcast'
author: ogirardot
type: post
date: 2014-11-27T14:00:00+00:00

publicize_linkedin_url:
  - 'https://www.linkedin.com/updates?discuss=&scope=33725914&stype=M&topic=5943735014897262592&type=U&a=oUEx'
publicize_twitter_user:
  - ogirardot
publicize_twitter_url:
  - http://t.co/gdeNIR2bu5
categories:
  - Apache Spark
  - BigData
tags:
  - Broadcast
  - HDFS

---
<!--more-->
> <p style="text-align:justify;">
>   <a title="Apache Spark" href="https://spark.apache.org" target="_blank">Apache Spark</a> est un moteur de calcul distribué visant à remplacer et fournir des APIs de plus haut niveau pour résoudre simplement des problèmes où Hadoop montre ses limitations et sa complexité.
> </p>
> 
> <p style="text-align:justify;">
>   Ce billet fait partie d'une série de billet sur Apache Spark permettant d'approfondir certaines notions du système du développement, à l'optimisation jusqu'au déploiement.
> </p>

<p style="text-align:justify;">
  Un des avantage principaux de Spark est sa capacité à être bien intégré à l'éco-système Scala/Java ou Python. C'est d'autant plus vrai en Scala car les méthodes principales attachées aux contextes Spark sont de la même forme qu'en Scala avec quelques améliorations (et le contexte distribué en plus) ex: <strong>map, flatMap, filter...</strong>
</p>

<p style="text-align:justify;">
  Cet avantage vient avec l'inconvénient qu'il est important de savoir quels objets/instances manipulées et dans quel contexte Spark ou Scala ces objets vont être utilisés. Si vous en doutez, voilà un petit exemple permettant de bien l'illustrer :
</p>

[code language=&#8221;scala&#8221;]  
val multiplier = 50

val data = sc.parallelize(1 to 10000)  
val result = data  
.map( _ * multiplier)  
.filter( _ > 1000 )  
.collect()  
.map( _ / 2 )  
.filter( _ < (20 * multiplier) )  
[/code]

<p style="text-align:justify;">
  Si on étudie cet exemple volontairement simpliste, les deux premières opérations <b>map </b>et <strong>filter </strong>s'appliquent sur un <strong><a title="Apache Spark - RDD API" href="https://spark.apache.org/docs/latest/api/scala/index.html#org.apache.spark.rdd.RDD" target="_blank">RDD[Int]</a> </strong>géré par Spark et vont donc s'exécuter dans un contexte parallélisé, ce n'est plus le cas dès l'appel à <strong>collect() </strong> qui va ramener la totalité des données traitées par les <strong>workers </strong>vers la mémoire du <strong>driver Spark</strong>. Ainsi les deux autres appels à <strong>map</strong> et <strong>filter </strong>vont s'appliquer sur une <a title="Scala API - List" href="http://www.scala-lang.org/api/current/index.html#scala.collection.immutable.List" target="_blank"><strong>List[Int]</strong></a> et donc font partie de la Standard Library de Scala.
</p>

<p style="text-align:justify;">
  Cet exemple a deux propriétés importantes, il permet de voir la confusion possible entre les appels Scala et Spark, mais surtout il permet de voir, avec le coefficient <strong>multiplier </strong>utilisé, qu'il est assez simple d'utiliser des <strong>valeurs Scala dans une closure envoyée à Spark.</strong>
</p>

<p style="text-align:justify;">
  La sérialisation des closures Scala vers les workers Spark méritera un article à lui tout seul et donc n'est pas l'objet de cet article, mais pour bien comprendre le problème qui nous intéresse il suffit de savoir qu'à <strong>chaque instance de la closure lancée par un worker contiendra une copie de la valeur utilisée.</strong> Ainsi si cette valeur correspond à une donnée un peu volumineuse cela devient rapidement inefficace et surtout dangereux pour l'utilisation mémoire de vos workers.
</p>

<p style="text-align:justify;">
  Heureusement Spark vient avec deux notions de <strong>variables partagées </strong>les Accumulateurs et variables <strong>Broadcastées,</strong> maintenant comme vous aurez deviné c'est cette dernière notion qui vient à notre secours.
</p>

<p style="text-align:justify;">
  En effet au lieu d'avoir autant de copie des valeurs dans les closures que nous avons d'appels dans les workers par celle-ci, il est possible d'utiliser la fonction de <strong>broadcast</strong><strong>()</strong> pour partager en lecture-seule cette valeur et ainsi n'avoir qu'une copie par noeud géré par le système.
</p>

<p style="text-align:justify;">
  Cette fonction n'est en revanche intéressante que pour partagé de grosses sources de données à travers les workers, par pour notre pauvre petit <strong>Int</strong> de <strong>multiplier</strong> dans l'exemple précédent, voilà comment l'utiliser :
</p>

[code language=&#8221;scala&#8221;]  
val largeKeyValuePair: Map[String, String] = ....

// broadcast this variable for workers to use it efficiently  
val bdLarge = sc.broadcast(largeKeyValuePair)

val data = sc.parallelize(1 to 10000)  
val result = data  
.map( item => (item, bdLarge.value.get(item.toString) )  
...  
[/code]

<p style="text-align:justify;">
  Pour résumé, le <strong>broadcast </strong> sert à n'envoyer qu'une seule fois une valeur assez large pour en valoir la peine. Maintenant votre question doit être &#8220;grosse comment&#8221; ?
</p>

<p style="text-align:justify;">
  L'Université de Berkeley (CA) a étudié la question dans la publication suivante sur les<a title="Broadcast performance for Apache Spark" href="http://www.cs.berkeley.edu/~agearh/cs267.sp10/files/mosharaf-spark-bc-report-spring10.pdf" target="_blank"> performances des différents algorithmes de broadcasting entre les noeuds</a> et pour vous la faire courte, le mécanisme standard de broadcasting de Spark <strong>Centralized HDFS Broadcast (CHB pour les intimes) </strong>donne ce genre de performance selon la taille des payloads :
</p>

<p style="text-align:justify;">
  <a href="https://ogirardot.wordpress.com/wp-content/uploads/2014/11/spark-broadcast-performance.png"><img loading="lazy" decoding="async" class="size-medium wp-image-1204 aligncenter" src="https://ogirardot.wordpress.com/wp-content/uploads/2014/11/spark-broadcast-performance.png?w=300" alt="spark-broadcast-performance" width="300" height="215" /></a>
</p>

<p style="text-align:justify;">
  <p style="text-align:justify;">
    Si vous voulez en savoir plus, j'organise avec Lateral Thoughts et Hopwork des formations Spark régulières, l'agenda est disponible ici : <a title="Lateral Thoughts - Formations" href="http://www.lateral-thoughts.com/training" target="_blank">http://www.lateral-thoughts.com/training</a>.
  </p>