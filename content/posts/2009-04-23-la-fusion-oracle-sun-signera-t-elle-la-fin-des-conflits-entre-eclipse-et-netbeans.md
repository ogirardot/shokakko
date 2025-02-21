---
title: La fusion Oracle – Sun signera-t-elle la fin des conflits entre Eclipse et Netbeans ?
author: ogirardot
type: post
date: 2009-04-23T18:40:00+00:00

blogger_blog:
  - readtfb.blogspot.com
blogger_author:
  - ssaboumhttp://www.blogger.com/profile/15320526353188755053noreply@blogger.com
blogger_permalink:
  - /2009/04/la-fusion-oracle-sun-signera-t-elle-la.html
original_post_id:
  - 20
categories:
  - Uncategorized

---
<!--more-->
Le point est le suivant, Oracle est une très grosse compagnie, dont le business model est moins orienté OpenSource qu'IBM, elle possède une base solide d'utilisateurs et des licenses très coûteuses qui lui assurent une base de revenus conséquente, mais qui possède à son actif peu de projet OpenSource.  
Au contraire de Sun qui pour ne pas les citer (en fait si...) coordonne quand même MySQL, OpenSolaris, OpenOffice, sans compter Java, et tous les produits OpenSource autour de ces produits phares.

Il serait trop long de s'attarder sur la totalité des possibilités d'évolution que va posséder Oracle avec ce rachat, mais j'aimerais m'attarder sur deux points, les conséquences possibles de cette fusion vis-à-vis d'OpenOffice.org et de Netbeans.  
Netbeans pour le rappeler est le principal concurrent d'Eclipse en terme d'environnement de développement intégré pour Java, Ms Visual Studio étant ... hors de question.

L'histoire veut que lors de la première mouture de Swing pour Java, IBM n'ait pas supporté la tête des interfaces légères que proposait alors Sun Microsystems. Ainsi après des discussions animées, IBM a décidé de se construire un nouveau toolkit graphique qui deviendra ce qu'utilise à l'heure actuelle la plateforme Eclipse : SWT (Standard Widget Toolkit).  
La particularité de ce toolkit est que tous les objets, même les plus simples comme les rectangles, ont été recodés, ainsi il n'existe aucune dépendance entre ces objets et les librairies standards (de facto ) de Java (AWT/Swing).

Puis le schisme s'est amplifié jusqu'a qu'à l'heure d'aujourd'hui où Swing possède une option permettant de retrouver des interfaces lourdes ( le graphisme étant alors propre au système d'exploitation utilisé et non plus indépendant) appelé le LookAndFeel.

Et Eclipse possède maintenant un framework libre autour de SWT permettant de créer des applications ayant les mêmes moutures qu'Eclipse (Eclipse RCP utilisé par exemple par Azureus/Vuze) , ainsi tout est bien dans le meilleur des mondes (tant qu'on ne veut pas faire communiquer les deux efficacement).

Seulement voilà, maintenant Oracle a racheté Sun Microsystems, et Oracle sans être fanatique de l'OpenSource, contribue de manière importante à Eclipse qui est beaucoup utilisé par ses clients ... La donne a changé, et il y a fort à parié que des changements de politique au sein de Sun pourrait voir la disparition de Netbeans ou une meilleure intégration de Netbeans et d'Eclipse, qui sait ?

Ne rêvons pas tout haut, mais vu que ces problèmes sont politiques, un peu de changement fera du bien. Il en va de même avec le projet OpenOffice.org car Sun tient/tenait le projet d'une main de fer, ne laissant pas le champ libre à la communauté pour beaucoup de développements, en enterrant par la même occasion beaucoup de problèmes ...

Je pense donc que beaucoup de bien peut sortir de cette évolution, espérons juste qu'Oracle n'essayera pas d'imposer son business model à une société qui a déjà une grande notoriété.

Äddi.