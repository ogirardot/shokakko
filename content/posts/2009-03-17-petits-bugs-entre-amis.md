---
title: Petits bugs entre amis …
author: ogirardot
type: post
date: 2009-03-17T19:19:00+00:00

blogger_blog:
  - readtfb.blogspot.com
blogger_author:
  - ssaboumhttp://www.blogger.com/profile/15320526353188755053noreply@blogger.com
blogger_permalink:
  - /2009/03/petits-bugs-entre-amis.html
original_post_id:
  - 16
categories:
  - Uncategorized

---
<!--more-->
Ce sujet, bien plus sérieux qu'il n'y paraît, lance pour moi le début d'un décompte. En effet il n'est pas rare de tomber sur le site voyages-sncf sur des petits bugs, des difformités de fonctionnement, ou même carrément des plantages bien génant. Les exemples sont multiples et je laisse chacun en apprécier la saveur devant son écran.

Mais pour qu'on replace le contexte, qui à mon avis a son importance, il y a peu sur le site de la SNCF, quand on arrivait sur la page d'acceuil où l'on pouvait directement commencer la reservation d'un train, on tapait son trajet et la date du jour était mise automatiquement, ainsi que l'heure.  
On était ensuite envoyé sur une interface plus complexe, où l'on pouvait mettre les modalités, carte 12/25 et autres réjouissances, pour enfin valider son trajet et rechercher son train parmis les choix disponibles.  
Ce protocole n'est néanmoins plus d'actualité, car quand on est sur la page d'acceuil et que l'on lance la recherche, on atteris maintenant directement sur la liste des trains disponibles. C'est à croire que quelqu'un s'est plaint du site de la sncf...  
Passons sur le coté mal-pratique de cette modification qui nécessite maintenant, pour quelqu'un qui ne connaît pas le site par coeur, de se tromper au moins une fois avant de reserver son train correctement, car non ce n'est pas là le but de cet article, mais regardez plutôt l'interface de la page d'acceuil ci-après :

[<img decoding="async" style="margin:0 10px 10px 0;float:left;cursor:pointer;width:320px;height:261px;" src="http://4.bp.blogspot.com/_qTrjuYjK_hI/Sb_4l_xfwFI/AAAAAAAAABY/kYcBS1-GoxA/s320/Image+4.jpg" border="0" alt="" />][1]

J'ai mis un trajet par défaut pour le test, mais au niveau de la date et de l'heure il y a quand même dû y avoir une perte de code quelque part...

En creusant un peu, on peut trouver que, d'une part ce bug ne se reproduit pas sur l'interface complète, et compte tenu de la manière dont le bug s'affiche, il s'agit d'un problème de javascript (petit oxymore...) dont voici la raison :

<pre>"#outward_date":function(o){
if(...){o.value="JJ/MM/AAAA";}
}</pre>

Je sais pas trop où voulait en venir le développeur, par contre je sais que la conséquence est qu'on arrive en laissant les paramètres à l'écran suivant :  
[<img decoding="async" style="margin:0 auto 10px;display:block;text-align:center;cursor:pointer;width:320px;height:221px;" src="http://1.bp.blogspot.com/_qTrjuYjK_hI/ScAPQQ35koI/AAAAAAAAABg/Cf7KjlXQ0h0/s320/Image+5.png" border="0" alt="" />][2]Donc voilà la conséquence de ce petit bug, mais l'intérêt n'est pas là, le compte à rebours est donc lancé, selon vous combien de temps ce bug va rester ???  
Les paris sont ouverts.

 [1]: http://4.bp.blogspot.com/_qTrjuYjK_hI/Sb_4l_xfwFI/AAAAAAAAABY/kYcBS1-GoxA/s1600-h/Image+4.jpg
 [2]: http://1.bp.blogspot.com/_qTrjuYjK_hI/ScAPQQ35koI/AAAAAAAAABg/Cf7KjlXQ0h0/s1600-h/Image+5.png