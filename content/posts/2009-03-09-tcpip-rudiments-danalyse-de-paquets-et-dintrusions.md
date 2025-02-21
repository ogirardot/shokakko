---
title: TCP/IP – Rudiments d’analyse de paquets et d’intrusions
author: ogirardot
type: post
date: 2009-03-09T19:47:00+00:00

blogger_blog:
  - readtfb.blogspot.com
blogger_author:
  - ssaboumhttp://www.blogger.com/profile/15320526353188755053noreply@blogger.com
blogger_permalink:
  - /2009/03/tcpip-rudiments-danalyse-de-paquets-et.html
original_post_id:
  - 14
categories:
  - Uncategorized

---
<!--more-->
Cet article est pour présenter un peu de manière bas niveau l'analyse des transaction TCP/IP, je n'ai malheureusement pas de livre d'environs 25 cm d'épaisseur sur mon bureau expliquant tcp/ip tout en prenant joyeusement la poussière, donc je vais me contenter de prendre pour exemple les communications basiques de mon propre ordinateur.  
Cet article va donc être rédigé avec l'aide de [tcpdump][1], dont j'ai mis un lien vers un site présentant quelques commandes.

<span style="font-size:100%;">Pourquoi TCPDUMP ?</span> et pas des outils plus évolué comme Wireshark ou d'autres analyseurs de packets qui &#8220;font le café&#8221; (...) ?

Principalement parce qu'il est plus simple d'analyser des transactions typiques pour pouvoir après comprendre mieux certains IDS (Intrusion Detection System) et savoir distinguer les fausses alertes des vrais .  
Donc pour replacer le contexte TCP/IP est un modèle permettant donc de gérer les communications entre ordinateur dans un réseau, on va principalement s'intéresser à TCP (Transmission Control Protocol) parce qu'il est au coeur du système.  
On met souvent en relation TCP avec UDP (User Datagram Protocol), si les deux protocoles ont la même fonction, on les utilise dans des cas différents, car en fait, sans rentrer dans le detail de l'empaquetage des données, UDP est plus rapide, mais moins sûr car il ne contient pas d'accusé de réception des données, alors que TCP permet une pertes de paquets beaucoup moins importante.

Commençons :

Tout d'abord chaque connection entre deux ordinateurs commence par l'ouverture d'une transaction appellé la <span style="font-style:italic;">3-Way Handshake</span> à savoir 3 communication SYN, SYN/ACK puis ACK dont on peut voir ci-après le rapport via TcpDump :

<pre><br />21:17:58.931082 IP 192.168.1.10.51572 &gt; 192.168.1.1.2555: S <br />1780814516:1780814516(0) win 8192<br /><br />21:17:58.931775 IP 192.168.1.1.2555 &gt; 192.168.1.10.51572: S <br />575797494:575797494(0) ack 1780814517 win 5840<br /><br />21:17:58.932395 IP 192.168.1.10.51572 &gt; 192.168.1.1.2555: . <br />ack 575797495 win 4380<br /><br /></pre>

Ainsi 192.168.1.10, ma machine, initie sa connection au routeur, 192.168.1.1, par un paquet SYN symbolisé par le flag S ayant pour numero de séquence 1780814516.  
Le routeur répond par un SYN ayant pour numero de séquence 575797494 avec un ACK accusant réception du paquet numero 1780814516 en disant que le paquet 1780814517 est prêt à être reçu.  
Suite à quoi ma machine répond en accusant réception au routeur du paquet 575797494, se disant prêt à recevoir le paquet 575797495.

Un point intéressant de cette séquence est qu'entre parenthèse dans chaque échange, à part le ACK simple, est spécifié le nombre de bits échangés, ici 0.  
C'est toujours le cas pour une poignée de main &#8220;amicale&#8221;, seulement ces bits de données ne sont souvent pas vérifiés par certains IDS, ainsi certains pirates en profitent pour faire transiter des données, qui ne seront pas vérifiées, un bon moyen de voir une discution déjà mouvementée.

<pre><br />21:17:58.933498 IP 192.168.1.10.51572 &gt; 192.168.1.1.2555: P <br />1780814517:1780814833(316) ack 575797495 win 4380<br /><br />21:17:58.934419 IP 192.168.1.1.2555 &gt; 192.168.1.10.51572: . <br />ack 1780814833 win 3456<br /></pre>

Voilà ce que l'on peut observer par la suite lors de l'établissement de transferts, avec le transfert de 316 bits et l'accusé de réception de ces données.

Les données ci-dessus ont été récupérés en utilisant la commande suivante, pour ne récuperer qu'une forme simple des discutions TCP :

<pre><br />sudo tcpdump -i  -nS tcp<br /></pre>



  * Attaques détectables par l'analyse de paquets :

Le <span style="font-style:italic;">TCP SYN Flood </span>est un type de DDoS (Distributed Denial Of Service), qui permet donc de paralyser une machine jusqu'a ce qu'elle soit incapable de délivrer le service qu'elle est censé apporté. Cette attaque est très vieille et ne fonctionne plus vraiment sur les machines moderne (en tout cas pas sans une armée de machines).

Elle consiste simplement dans le fait que lors de l'initialisation d'une connection le zombie &#8220;oublie&#8221; l'accusé de réception ACK permettant de conclure la connection.  
En faisant cela, le zombie reste dans la file d'attente de la machine qui peut avoir une capacité limitée et une inertie très grande.  
Les anciennes machines avait une pile de 8 clients possibles avec un temps d'attente avant de considéré le client comme invalide de 3 minutes, il n'y avait pas tellement besoin de puissance pour planter un serveur.  
On peut réaliser cette attaque soit en ommettant soit même l'envoi du ACK, soit en spoofant (falsifiant) l'adresse d'envois, ainsi le ACK ne sera jamais reçu, par exemple avec notre routeur de tout à l'heure (en ayant toujours l'adresse 192.168.1.10) :

<pre><br />21:17:58.931082 IP 192.168.1.13.51572 &gt; 192.168.1.1.2555: S <br />1780814516:1780814516(0) win 8192<br /><br />21:17:58.931775 IP 192.168.1.1.2555 &gt; 192.168.1.13.51572: S <br />575797494:575797494(0) ack 1780814517 win 5840<br /><br />..........</pre>

Une autre attaque plus <span style="font-style:italic;">marrante </span>existe et pour laquelle il est intéressant de la détecter dans les paquets assez tôt, on l'appelle <span style="font-style:italic;">coke </span>et elle consistait à saturer les logs anti-hacker de WinNt en forçant les paquets à contenir le maximum de bits possibles, autant dire qu'a cette époque les capacités des serveurs étant bien moindre, cette attaque était très efficace.

Une dernière plus technique consiste à lever un drapeau MF = More Fragmentation, dans les paquets. La fragmentation permet de transférer des plus petites quantité d'un même paquets de données, les fragments étant numérotés.  
Mais il suffit de lever ce drapeau et d'envoyer sans relache le même identifiant de fragment pour que le système attende une fin de transmission qui n'arrivera jamais.

Enfin juste pour le memorandum, même si un jour ça peut s'avérer utile, voilà une liste de commandes de filtrage que l'on peut appliquer à tcpdump pour reconnaitre certaines attaques, mais rien ne remplace un oeil humain.  
[http://acs.lbl.gov/~jason/tcpdump.filters][2]

 [1]: http://dmiessler.com/study/tcpdump/
 [2]: http://acs.lbl.gov/%7Ejason/tcpdump.filters