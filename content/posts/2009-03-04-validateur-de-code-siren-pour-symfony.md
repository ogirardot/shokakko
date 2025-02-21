---
title: Validateur de code SIREN pour symfony
author: ogirardot
type: post
date: 2009-03-04T20:20:00+00:00

blogger_blog:
  - readtfb.blogspot.com
blogger_author:
  - ssaboumhttp://www.blogger.com/profile/15320526353188755053noreply@blogger.com
blogger_permalink:
  - /2009/03/validateur-pour-code-siren-pour-symfony.html
original_post_id:
  - 13
categories:
  - Uncategorized

---
<!--more-->
Dans la série des codes qui peuvent servir, voilà une classe PHP à copier dans le dossier lib de Symfony, elle permet, via l'algorithme de Luhn, de vérifier si un Code SIREN est valide dans un formulaire.

<pre><br />/**<br />* Classe implémentant une vérification du code Siren par l'algorithme de Luhn<br />* <br />*/<br /><br />class sfCodeSirenValidator extends sfValidator<br />{<br /><br />public function execute($value, $error)<br />{<br /> //if so the length of the string given is incorrect<br /> if(strlen(trim($value))!=9){<br />  $error=$this-&gt;getParameter('sirenlength_error');<br /> <br />  return false;<br /> }<br /><br />      $odd = !strlen($value)%2;<br />      $sum = 0;<br />      for($i=0;$i &lt; strlen($value);++$i) {<br />          $n=0+$value[$i];<br />          $odd=!$odd;<br />          if($odd) {<br />              $sum+=$n;<br />          } else {<br />              $x=2*$n;<br />              $sum+=$x&gt;9?$x-9:$x;<br />          }<br />      }<br />      if(($sum%10)==0){<br />       return true;<br />      } else {<br />       //the siren code is not a proper formatted one<br />       $error=$this-&gt;getParameter('siren_error');<br />      <br />       return false;<br />      }<br /><br />}<br /><br />public function initialize ($context , $parameters = null ){<br /><br />  //Initialize parent<br />  parent::initialize($context);<br /><br />  $this-&gt;setParameter('sirenlength_error' , 'Incorrect size for a siren code ');<br />  $this-&gt;setParameter('siren_error' , 'This is not a valid siren code');<br /><br />  //Set parameters<br />  $this-&gt;getParameterHolder()-&gt;add($parameters);<br /><br />  return true;<br />}<br /><br />}<br /></pre>

Son utilisation est simple via un fichier <span style="font-style:italic;">validate.yml</span> de la façon suivante en le couplant à une vérification du nombre:



<pre><br />fields:<br />  codesiren:<br />    sfNumberValidator:<br />      nan_error: Le code siren est un numéro<br />    sfCodeSirenValidator:<br />      siren_error:      Le code siren est invalide<br />      sirenlength_error: La taille du code siren entré est incorrecte<br /></pre>

Ce validateur permet ainsi d'être sûr de la validité d'un code SIREN et donc de l'identité de l'entreprise que l'on cherche à ajouter.