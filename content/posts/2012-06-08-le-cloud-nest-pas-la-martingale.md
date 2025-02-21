---
title: Le Cloud n’est pas la martingale
author: ogirardot
type: post
date: 2012-06-08T16:01:45+00:00

original_post_id:
  - 860
categories:
  - TechZone
tags:
  - APPARTINFO
  - cloud
  - NoSQL
  - scaling

---
<p style="text-align:justify;">
  Je vois de plus en plus de devs et autre afficionados technolo-geek commencer à ne jurer que par le cloud, alors à défaut de présenter une vision partiale et totalitairement contre, ce que je ne suis pas, j'aimerais bien tempéré un peu ces ardeurs.
</p>
<!--more-->

<p style="text-align:justify;">
  En même temps... Fais pas bon critiquez le cloud ces temps-ci, récemment c'est <a href="http://twitter.com/zeeg" target="_blank">David Cramer</a> <em>(créateur de Sentry et dev chez <a href="http://disqus.com/" target="_blank">Disqus</a> - la plus grosse application Django du monde)</em> qui s'était exercer avec un article que je vous conseille comme annexe &#8220;<a href="http://justcramer.com/2012/06/02/the-cloud-is-not-for-you" target="_blank">The Cloud is not for you</a>&#8220;. C'est mal parce qu'<strong>enfin avec le cloud (et le NoSQL), on nous vend du rêve !</strong>
</p>

<p style="text-align:justify;">
  <strong></strong><br /> Pour résumé, si vous ne le saviez pas déjà, avec le cloud vous allez :<br /> - pouvoir &#8220;scaler&#8221; jusqu'à l'infini votre application pour gérer des milliards de requêtes par secondes;<br /> - pouvoir enfin &#8220;vous abstraire&#8221; des machines, des base de données (<em>ORM Mon Amour</em>), et de toutes ces choses &#8220;sales&#8221;;<br /> - et avec des services comme Amazon vous allez atteindre la haute disponibilité à 99.99999999999999% le tout en ayant &#8220;que&#8221; à mettre la main au portefeuille;
</p>

<p style="text-align:justify;">
  J'en passe et des meilleurs... Je vais mettre un peu mon grain de sel (ça donne toujours du gout ! et j'espère que ça sera utile pour certains !) parce qu'en tant que dev et ops d'<a href="http://www.appartinfo.com" target="_blank">APPARTINFO</a>, la startup que nous construisons depuis bientôt 2 ans, j'ai un petit parti pris dans cette histoire.
</p>

<p style="text-align:justify;">
  En tant que développeur, et en tant que personne cherchant à faire notre travail, je considère que nous ne sommes pas là que pour développer, nous construisons un produit, une expérience utilisateur, une utilisation des resources mises à notre dispositions, nous rendons un service et construisons un outil.
</p>

<p style="text-align:justify;">
  Ce que nous vend le cloud, et qu'à très bien illustrer Nicolas Martignole dans &#8220;<a href="http://www.touilleur-express.fr/2012/06/02/le-cloud-pour-le-developpeur/" target="_blank">Le Cloud pour le développeur</a>&#8220;, c'est pour moi un rêve pourri, une contre-utopie où le développeur pourra enfin rester totalement dans sa cave, à faire couche d'abstraction sur couche d'abstraction pour enfin ne plus avoir de lien avec ni la machine, ni le métier. Youhou !
</p>

<p style="text-align:justify;">
  Voilà quelques vérités que je trouve importante, et sur lesquelles je rejoint David Cramer :
</p>

<p style="text-align:justify;">
  - Le Cloud (version Amazon, Heroku etc.) donne juste l'illusion de pouvoir <strong>scaler </strong>une application en un rien de temps. Le Cloud permet, c'est vrai, de dépenser de l'argent en rajoutant des machines, mais au fond dans l'optique de <strong>scaler </strong>une application, on a rarement besoin de pouvoir rajouter 50 machines dans une demi heure !
</p>

<p style="text-align:justify;">
  Clairement il faut distinguer les utilisations, <strong>amazon trouve sa place dans les besoins de puissances de calculs instantanées</strong>, mais à moins d'être le New York Times et d'avoir besoin de générer en 5 minutes 10 Milliards de PDF, ce n'est pas ce qui va gérer vos problèmes de charges en tout cas pas les miens, ni ceux de <a href="http://disqus.com/" target="_blank">Disqus</a>.
</p>

<p style="text-align:justify;">
  - C'est la chose la plus stupide au monde que de vouloir s'abstraire à un niveau supérieur des machines sur lesquelles toute application repose. <strong>Java</strong> n'est pas vraiment <em>&#8220;Compile Once, Run Everywhere&#8221; </em> et <strong>C/C++</strong> est encore le langage le plus utilisé quand on cherche la performance. Sans compter que tous nos composants critiques, <strong>Base de données, Caches, Queue Managers, Reverse Proxy, </strong><strong>Serveurs d'applications </strong> et surtout le <strong>système d'exploitation</strong>, tous s'appuient sur des particularité, des routines et des algorithmes au plus proche de la machine. Même l'utilisation des B-Tree pour les systèmes de fichiers et bases de données est privilégié en partie à cause de l'utilisation de <a href="http://en.wikipedia.org/wiki/Cylinder-head-sector" target="_blank">disques dur à cylindres</a>...
</p>

<p style="text-align:justify;">
  Tout ça pour dire qu'encore une fois, on vend aux développeurs et aux décideurs ce qu'ils adorent, <strong>une couche d'abstraction supplémentaire</strong> permettant de se dire qu'enfin on va pouvoir se passer d'admin sys <em>(c vrai que c encore des gens avec qui il faut discuter 🙁 ... ), </em>on va aussi pouvoir se passer de comprendre les protocoles HTTP/TCP/WSGI/... ou de comprendre que quand la RAM est pleine, une application swap et que quand la swap est pleine... ben tout crash...
</p>

<p style="text-align:justify;">
  Sous le pretexte de la &#8220;<strong>Separation of Concerns</strong>&#8221; on dit au développeur, <strong>&#8220;toi tu dev et le reste c'est pas tes affaires&#8221;</strong>. Seulement un développeur qui ne s'occupe pas de sa mise en production, des performances et de l'impact de ses process sur les machines, de la sécurité de celles-ci, pour moi, il fait mal son travail.
</p>

<p style="text-align:justify;">
  Est ce que ceux qui défendent le Cloud coûte que coûte ont aussi bien compris qu'alors, on ouvrait tout les process critiques, les bases de données, les caches etc... sur Internet ?
</p>

<p style="text-align:justify;">
  On va bien s'amuser quand une série de failles sera utilisées sur les bases de données &#8220;enfin accessibles&#8221;. Je vois même pas pourquoi je parle au future, <a href="http://blog.linkedin.com/2012/06/07/taking-steps-to-protect-our-members/" target="_blank">je suis sûr que linked voit de quoi je parle</a>.
</p>

<p style="text-align:justify;">
  Voilà c'était mon petit grain de sel,
</p>

<p style="text-align:justify;">
  <em>Vale</em>
</p>