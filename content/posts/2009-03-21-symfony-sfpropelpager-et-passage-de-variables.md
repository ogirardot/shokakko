---
title: Symfony, sfPropelPager et passage de variables
author: ogirardot
type: post
date: 2009-03-21T22:13:00+00:00

blogger_blog:
  - readtfb.blogspot.com
blogger_author:
  - ssaboumhttp://www.blogger.com/profile/15320526353188755053noreply@blogger.com
blogger_permalink:
  - /2009/03/symfony-sfpropelpager-et-passage-de.html
original_post_id:
  - 17
categories:
  - Uncategorized

---
<!--more-->
Dans le cadre d'un projet, j'ai dû réaliser une pagination des résultats d'une recherche qui possédait de nombreux critères. Or le fait est que je n'avais jamais réalisé un Pager avec Symfony, car je me suis toujours basé sur le listing généré par l'Admin Generator...

Ce fut donc une expérience intéressante que de réaliser la pagination en utilisant le sfPropelPager de Symfony comme base. Donc pour replacer le contexte, on effectue un sélection sur une table individu avec une longue liste de Criteria et on fait :

<pre>$this-&gt;pager = new sfPropelPager('Individu', 20);
$this-&gt;pager-&gt;setCriteria($c);

$this-&gt;pager-&gt;setPage($this-&gt;getRequestParameter('page',$this-&gt;getUser()-&gt;getAttribute('page',1,'sf_admin/annuaire')));

$this-&gt;pager-&gt;init();
$this-&gt;recherche=$recherche;

// save page
if ($this-&gt;getRequestParameter('page')) {    $this-&gt;getUser()-&gt;setAttribute('page', $this-&gt;getRequestParameter('page'),'sf_admin/annuaire');}</pre>

On suppose que l'on a nommé les variables &#8220;recherche[...]&#8221; dans le formulaire de recherche, ainsi Symfony va mettre les résultats dans une matrice associative &#8220;recherche&#8221; que l'on va récupérer en paramètre. Je laisse le soin au lecteur de consulter la documentation de symfony concernant l'utilisation du pager pour gérer la navigation et autre.

Une fois la recherche effectuée et le Pager initialisé se pose un problème, Comment conserver les paramètres de la recherche page après page ????

Sans plus aller chercher en avant dans la totalité des solutions possibles, car j'en ai tenté plusieurs dont le passage en GET des paramètres dans l'url à chaque page... Voilà le moyen qu'utilise Symfony et qui permet de contourner tout le principe de passage de variable d'une action à un template (du Controler à une Vue) :

Il est en fait possible d'utiliser des Namespace, qui a contrario de C++ ne permettent pas de définir des classes, mais servent plus d'entrepôts pour stocker des variables. Il est ainsi possible dans notre cas de stocker la recherche et de la repasser de page en page, ainsi en début de méthode de recherche on écrit les lignes suivantes :

<pre>if($this-&gt;getRequestParameter('recherche')){$recherche = $this-&gt;getRequestParameter('recherche');

$this-&gt;getUser()-&gt;getAttributeHolder()-&gt;removeNamespace('sf_admin/annuaire');

$this-&gt;getUser()-&gt;getAttributeHolder()-&gt;removeNamespace('sf_admin/annuaire/recherche');

$this-&gt;getUser()-&gt;getAttributeHolder()-&gt;add($recherche, 'sf_admin/annuaire/recherche');}

$recherche = $this-&gt;getUser()-&gt;getAttributeHolder()-&gt;getAll('sf_admin/annuaire/recherche');</pre>

Cette simple commande en début de code permet d'une part de récupérer les paramètres de la recherche via <span style="font-style:italic;">getRequestParameter</span> lors du chargement de la première page et de les sauvegarder dans le namespace, et sinon si aucun paramètre n'est passé, de reprendre dans le namespace les paramètres.

Ainsi on court-circuite le passage de variables à une template qui ne va pas les modifier sauf en cas de nouvelle recherche.