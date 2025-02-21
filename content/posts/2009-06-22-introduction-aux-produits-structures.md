---
title: Introduction aux produits structurés
author: ogirardot
type: post
date: 2009-06-22T21:25:05+00:00

original_post_id:
  - 97
categories:
  - Uncategorized

---
<!--more-->
Je vais commencer une petite série d'article sur les produits structurés qui, bien que n'étant pas une de mes spécialités (loin de là) sont autour de mon travail. Mais avant ça reprenons déjà la base de vocabulaire et de principes qui vont aider à comprendre les produits que je vais essayé de présenter.

Tout d'abord la base des marchés financier est basé sur le concept d'achat et de vente, ça normalement tout le monde connaît, le but principal des acteurs de ces marchés est de limiter leur exposition aux variations du marché, donc de limiter **le risque** tout en faisant ce qu'ils ont à faire.  
Ainsi un certain nombre de produits ont été créés pour permettre à certains acteurs de limiter leur risque : on appelle le **spot** le prix à un temps **t** d'un actif sur le marché, comme il est soumis au marché, il peut varier grandement.

Par exemple un agriculteur pour limiter son risque pourra vouloir négocier avant la saison de la récolte avec ses futurs acheteurs un prix à une date précise, plutôt que de devoir vendre au prix du marché au temps t.

Les contrats **futures/forward** ont été concus pour ça, ils se font entre deux parties définissant un produit à un prix pour une date précise avec un lieu de livraison (et parfois des standards de qualité quantifiables), ainsi l'agriculteur est sûr de vendre à tel prix et l'acheteur d'acheter à tel prix, si le spot augmente peut-être que l'agriculteur aura _perdu_ de l'argent et vice et versa pour l'acheteur, mais dans les deux cas les deux ont **couvert** leur risque de payer plus cher. _( ces contrats sont beaucoup utilisés par les coopératives aux états unis, mais très peu en France)..._

Pareillement les **options** sont des produits financiers qui permettent d'acheter un droit d'achat ou de vente (qui peut être exercé ou pas), on dit ainsi &#8220;dans 3 mois j'achèterais 3 tonnes de blé à 50k euros (ou pas)&#8221;,l'option permet à son possesseur si au bout de 3 mois 3 tonnes de blé (au prix du marché) valent 40k de ne pas utiliser son droit d'achat préférentiel, et d'acheter plutôt sur le marché.

Par contre si le **spot** dépasse le **strike** ( valeur à l'échéance du contrat = 50k), il aura la possibilité d'acheter à un prix inférieur au marché (et réalisera donc un bénéfice). Les options d'achat sont appelés des **call** et les options de ventes des **put,** ils possèdent une propriété intéressantes appelée la **parité call/put** ainsi un put et un call sont équivalents :

En notant $$K$$ le **strike** (ou prix d'exercice), $$C$$ le call et $$P$$ le put, ainsi que $$r$$ le taux d'intérêt sans risque (e.g. taux d'épargne dans une banque), $$T$$ la durée et $$S$$ le spot, on peut écrire

$$!C - P = S - K * e^(-rT)$$

D'où l'équivalence relative entre un call et un put.

Si les métaphores que j'utilise sont principalement autour de denrées physique, tous ces contrats s'appliquent aujourd'hui sur tout types de titres (actions, obligations, taux de changes, taux d'intérêts, indices climatiques, matières premières...).

Les produits structurés sont construit avec pour base ce genre de produits, il resterai les obligations et les actions, mais elles sont plus connues. Quant au prochain article, il introduira les **obligations zero-coupon** qui servent de base aux produits structurés _&#8220;de préservation du capital&#8221;._