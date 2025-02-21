---
title: Migration d’un projet Symfony 1.0 vers 1.2
author: ogirardot
type: post
date: 2009-02-01T19:45:00+00:00

blogger_blog:
  - readtfb.blogspot.com
blogger_author:
  - ssaboumhttp://www.blogger.com/profile/15320526353188755053noreply@blogger.com
blogger_permalink:
  - /2009/02/migration-dun-projet-symfony-10-vers-12.html
original_post_id:
  - 4
categories:
  - Uncategorized

---
<!--more-->
Malgré l'évident désintérêt d'un <span style="font-style:italic;">non-geek </span>face à un titre aussi passionnant, je tiens à laisser une trace autre que celle de mon expérience de cette transition difficile que j'ai dû réaliser.

Bien sûr chaque expérience est à prendre au cas par cas et toute expérience est la bienvenue pour compléter ce tutorial, car je ne peux pas inventé des bugs auxquels je n'ai pas été confronté.

Alors tout d'abord il y a bien sûr une différence fondamental d'organisation au niveau de la racine du projet avec l'ajout dans <span style="font-style:italic;">/config </span>du fichier <span style="font-style:italic;">ProjectConfiguration.class.php...<br /> </span>Aussi je conseille de repartir d'un projet symfony 1.2 vide et de copier les librairies importantes (de la couche objet et des validateurs) ainsi que les dossiers des modules à leur place.

Ensuite plusieurs bugs peuvent apparaître lors du basculement, il faut donc s'assurer le plusieurs choses :

  * le mode compatibilité <span style="font-style:italic;">compat_10 </span>de symfony 1.2 doit être activé
  * il faut revérifier les signatures de toutes les méthodes qui ont été surchargé dans la couche objet, car elles ont changés, par exemple: 
    <pre>//symfony 1.0
public function delete( $con = null ) { };
//symfony 1.2
public function delete(PropelPDO $con = null ){}</pre>

  * et reprendre les gestions d'erreurs car de nombreuses méthodes sont devenue <span style="font-style:italic;">deprecated :</span>

  * les notifications avec <span style="font-style:italic;">flash </span>sont aussi à reprendre :
<pre>//symfony 1.0
$this-&gt;setFlash('notice', 'Your modifications have been saved');
//symfony 1.2
$this-&gt;getUser()-&gt;setFlash('notice', 'Your modifications have been saved');</pre>

  * et les plugins (point sur lequel je ne vais pas beaucoup m'attarder) doivent être étudier selon leur compatibilité avant. Par exemple le nouveau plugin integré de base <span style="font-style:italic;">Protoculous </span>contient de base les librairies javascript Prototype et script.a.culo.us

Cette ensemble de vérification n'est pas exhaustif mais permet de bien prendre en compte certaines des préocupations que l'on doit avoir à l'esprit lors de tels basculements.

N'hésitez pas si vous avez d'autres retours d'expérience à commenter, le blog est fait pour ça.