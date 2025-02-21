---
title: 'Langage de programmation : Comme si les performances comptaient …'
author: ogirardot
type: post
date: 2009-05-13T19:42:00+00:00

blogger_blog:
  - readtfb.blogspot.com
blogger_author:
  - ssaboumhttp://www.blogger.com/profile/15320526353188755053noreply@blogger.com
blogger_permalink:
  - /2009/05/langage-de-programmation-comme-si-les.html
original_post_id:
  - 25
categories:
  - Uncategorized

---
Comme j'anticipe le gros troll qui pourrait découler d'un tel titre, je vais préciser ma pensée : Je ne veux pas dire qu'il ne faut pas optimiser son code autant que faire se peut et qu'il n'est pas nécessaire de toujours réfléchir au plus performant.
<!--more-->
Par contre, je pense qu'il est nécessaire de se recentrer sur le principal, car dans la guerre des langages de programmation (Python, C/C++, Java, Cobol, Natural, Erlang, Lisp, OCamel, PHP (euhh... non, pas php...), etc ...), on attaque tout le temps les uns parce qu'ils sont objets, les autres parce qu'ils sont moins performants etc...  
Mais le problème principal n'est pas là, il est dans la capacité de tout un chacun de maintenir du code en l'état ou de l'améliorer, et voilà où je veux en venir dans cet article, l'exemple de base étant le combat Java/C++.

On privilégie souvent C++ à Java car on maitrise mieux la mémoire avec C++ : mais si un développeur recherchait la performance, est ce qu'il utiliserait vraiment C++ ?  
En termes de chargements et de dump de base de données dédiées, Cobol est un standard de rapidité et de fiabilité... et pourtant je n'en ferai pas pour tout l'or du monde si j'avais un projet à commencer.  
Ici et dans 99 % des cas, le principal problème d'un code &#8220;pensé&#8221; est sa lisibilité et sa maintenabilité. Très peu de personnes ont besoin d'être des programmeurs de génie pour écrire du code, et de manière plus pertinente, personne <span style="font-weight:bold;">ne doit avoir besoin</span> d'être un génie pour pouvoir repasser derrière. Si un code n'est pas compréhensible au premier coup d'oeil, c'est du temps perdu qui aurait pu être utilisé à l'améliorer ou à étendre ses fonctionnalités.

Après il existe bien sûr des concours [d'obfuscation de code][1] mais il est rare que la personne qui doit, un jour, repassé derrière trouve assez de force en elle pour saisir tout l'humour de la situation...

Voilà pour illustrer ce propos deux extraits de code source, le premier issu de java.util.ArrayList, et le deuxième... je vais vous laissez voir :

<pre><br />   /**<br />    * Returns <tt>true</tt> if this list contains no elements.<br />    *<br />    * @return <tt>true</tt> if this list contains no elements<br />    */<br />   public boolean isEmpty() {<br />       return size == 0;<br />   }<br /><br />   /**<br />    * Returns <tt>true</tt> if this list contains the specified element.<br />    * More formally, returns <tt>true</tt> if and only if this list contains<br />    * at least one element <tt>e</tt> such that<br />    * <tt>(o==null ? e==null : o.equals(e))</tt>.<br />    *<br />    * @param o element whose presence in this list is to be tested<br />    * @return <tt>true</tt> if this list contains the specified element<br />    */<br />   public boolean contains(Object o) {<br />       return indexOf(o) &gt;= 0;<br />   }<br /><br />   /**<br />    * Returns the index of the first occurrence of the specified element<br />    * in this list, or -1 if this list does not contain the element.<br />    * More formally, returns the lowest index <tt>i</tt> such that<br />    * <tt>(o==null ? get(i)==null : o.equals(get(i)))</tt>,<br />    * or -1 if there is no such index.<br />    */<br />   public int indexOf(Object o) {<br />       if (o == null) {<br />           for (int i = 0; i &lt; size; i++)<br />                 if (elementData[i]==null)<br />                     return i;<br />         } else {<br />             for (int i = 0; i &lt; size; i++)<br />                 if (o.equals(elementData[i]))<br />                     return i;<br />         }<br />         return -1;<br />     }<br /></pre>

Et voilà l'image du deuxième, c'est un code en C vainqueur du concours d'obfuscation en 1986

[<img decoding="async" style="margin:0 auto 10px;display:block;text-align:center;cursor:pointer;width:320px;height:145px;" src="http://4.bp.blogspot.com/_qTrjuYjK_hI/Sgst_gxgXlI/AAAAAAAAACo/hPtMChCK6JQ/s320/marshall.png" alt="" border="0" />][2]édité avec VIM bien sûr, pour des questions d'hygiène.  
Je passe rapidement sur la lisibilité de chacun de ces codes, principalement parce qu'on voit clairement les différences, mais si l'on doit retenir une chose de ces exemples, c'est qu'un bon code est toujours commenté avec plus de lignes qu'il ne contient de lignes de codes (attention quand même à préciser le but du programme plutôt que tomber dans le descriptif du code...).

C'est une exemple, car la Javadoc et les outils développés pour Java ainsi que les analyseurs de code comme [FindBugs][3] <span style="text-decoration:underline;"></span>et les formatteurs intégré dans les Environnement de développements comme Eclipse, permettent à tout un chacun de faire du code propre, lisible pour qu'ainsi on ne se retrouve pas avec des bouts de code impénetrables au sein d'une grosse application.

Alors même si le dernier exemple est un peu tiré par les cheveux, j'ai déjà eu à faire avec du code codé avec les pieds par des gens qui ne savaient pas ce qu'ils voulaient.  
Et même si la performance est quelque chose de très séduisant à atteindre, compte tenu des évolutions de puissance du matériel et de la pérennité actuelle des applications, les langages objets en général et Java/C++ plus particulièrement sont des briques très utiles pour ne jamais avoir à réinventer la roue et ne pas refaire tout les deux ans des programmes avec à chaque fois un point de vue différent.

 [1]: http://en.wikipedia.org/wiki/Code_obfuscation
 [2]: http://4.bp.blogspot.com/_qTrjuYjK_hI/Sgst_gxgXlI/AAAAAAAAACo/hPtMChCK6JQ/s1600-h/marshall.png
 [3]: http://findbugs.sourceforge.net/