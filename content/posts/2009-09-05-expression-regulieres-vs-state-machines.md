---
title: Expression régulières Vs State Machines
author: ogirardot
type: post
date: 2009-09-05T15:00:25+00:00

original_post_id:
  - 188
categories:
  - Java

---
<!--more-->
Les expressions régulières sont un outil très pratique, voire même la martingale dans beaucoup de situations. Seulement dans le cas (improbable) où vous deviez reprendre un code un peu comme celui là :

<pre>public static boolean isPrime(String number) {
// compiling the regexp
Pattern p = Pattern.compile("/^1?$|^(11+?)1+$/"); 

// creating the matcher
Matcher m = p.matcher(number); 

boolean isPrime = m.find();

return isPrime;
}</pre>

Il n'y a que deux solutions vraisemblablement utilisée, la première consistant à forwarder le boulot à quelqu'un de clairement &#8220;plus compétent&#8221; que vous (genre un stagiaire...), soit à tout ré-ecrire de manière plus &#8220;compréhensible&#8221; (où en gros vous allez refaire le travail, seulement comme c'est vous qui allez le faire la regexp sera &#8220;nécessairement plus claire&#8221; que celle de votre prédécesseur...).

Alors je dis NON, ce n'est pas une fatalité... lol

Le problème est simple, les regexp sont souvent difficilement maintenables. Et la solution est encore plus simple, arréter d'en faire pour n'importe quoi, surtout quand elles ne sont pas nécessaires, et où elles compliquent la tâches plus qu'autre chose... Personnellement j'ai fini par être convaincu qu'il est souvent plus simple de faire une simple machine à états, mais ne vous inquiétez pas je vais m'expliquer par un petit exemple...

Certains \***\****, de la race de ceux qui adorent dire &#8220;mais pourtant ça marche dans mon Excel&#8221;, adorent donner comme source de donnée pour des transferts des fichiers CSV (Comma Separated Values) suivant un format très drole. Le plus ironique j'ai trouvé avait pour habitude de donner des lignes comme ça :

_**&#8220;Ccy Name, For the current date&#8221;, 28-Sep-2009, 456.75, (248.56), &#8220;134,546,678.15&#8221;**_

Avec la virgule comme séparateur de colonne, des parenthèses pour représenter des nombres négatifs, et des virgules (toujours !!) comme séparateur des milliers, et un point pour les nombres à virgules... Mais bien sûr comme ça pose parfois des problèmes d'avoir pleins de virgules comme ça, Excel a décidé de mettre des guillemets autour des chiffres à problèmes (donc pas tout le temps).

Alors comment vous parseriez cette ligne avec une Expression régulière ?

Je ne vous dirait pas la regexp que j'ai obtenu pour des trucs pareils.... mais j'attends avec impatience vos commentaires, si le coeur vous en dit 😉

Donc finalement lors du développement du parseur, j'ai finalement conçu une méthode _public String trashAnyUnWantedComma(String line)_ pour gérer ces cas pourris :

<pre>public String trashAnyUnWantedComma(String line) {
		StringBuilder result = new StringBuilder();
		boolean commaSeparatedFieldReached = false;

		for (char c : line.toCharArray()) {
			if (c == '"') {
				commaSeparatedFieldReached =
					(commaSeparatedFieldReached)? false : true;

			} else if (commaSeparatedFieldReached) {
				if (c == ',') {
					continue;
				}
			}
			result.append(c);
		}
		return result.toString();
}</pre>

ce qui avec la bonne dose de commentaire, permet d'avoir une fonction simple qui fait le travail et qui reste compréhensible.

Voilà, KIS : Keep It Simple