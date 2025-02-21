---
title: Modélisation de la C.A.F par des Threads asynchrones
author: ogirardot
type: post
date: 2009-07-17T22:29:17+00:00

original_post_id:
  - 152
categories:
  - Java
tags:
  - CAF
  - DeadLock
  - divarvel
  - LiveLock
  - Threads

---
<!--more-->
Sur une idée originale de <a title="Eklablog by Divarvel & co" href="http://www.eklablog.com/" target="_blank">Divarvel</a>, qui a un jour twitté que le meilleur exemple pour décrire les Threads asynchrones était la C.A.F... et surtout grâce aux C.A.F de Loire-Atlantique et de Moselle qui pour une fois m'ont donnée un bon exemple que je vais retranscrire de la manière la plus fidèle possible.

> <span style="color:#ff0000;">!!!! CET ARTICLE N'EST SURTOUT PAS UN PAMPHLET CONTRE LES C.A.F. QUI FONT UN TRES BON TRAVAIL, C'EST JUSTE POUR L'EXEMPLE PEDAGOGIQUE ET TOUTES RESSEMBLANCES AVEC DES SITUATIONS EXISTANTE SONT BIEN SÛR TOTALEMENT INVOLONTAIRE !!!!</span>

Alors tout d'abord on va définir ce qu'est une C.A.F. (ou Caisse d'Allocation Familiale). Au fond c'est une organisme conçu pour aider, en versant de l'argent, les personnes qui en ont besoin, mais ce n'est pas cette définition qui nous intéresse, plutôt une définition en terme d'objet :

<pre>import java.math.BigDecimal;

public interface CAF {

	/**
	 * Pour accéder au dossier sur internet
	 *
	 * @param numeroAllocataire
	 * @param mdp
	 */
	public void accesDossierAllocataire(Integer numeroAllocataire, String mdp);

	/**
	 * Pour parler à un conseiller de la C.A.F. de son dossier par téléphone
	 *
	 * @param numeroAllocataire
	 * @param mdp
	 */
	public void accesConseillerCAF(Integer numeroAllocataire, String mdp);

	public BigDecimal getAllocations(...); // BigDecimal ... un peu exagéré

	public void recevoirDossierTransféré(Dossier dossierAllocataire);

	public Dossier envoyerDossierToCAF(String autreCAF);
}</pre>

Ainsi voilà l'interface qui caractériserait une C.A.F, (en un peu simplifiée... mais le coeur y est),  bien sûr l'implémentation des méthodes est propre à chaque C.A.F., c'est d'ailleurs pour ça qu'elles ont toutes des sites internet différents...  <span style="color:#000000;"></span>Alors prenons 3 entités différentes, un étudiant qui déménage de Nantes vers Metz, la caf de loire-atlantique, la caf de la moselle et voyons comment ils vont interagir ensemble :

L'étudiant, tout d'abord, il est bête et feignant, il ne sait que déménager et emménager, mais il pense quand même à faire ses déclarations de C.A.F :

<pre>public class Etudiant{

	public void demenage(){
		// casse des meubles
		...
		// oublie des affaires
		...
		// fait chier ses parents...
		...
		// ah oui !! et appelle sa CAF pour dire qu'il déménage
		appellerCAFpourDemenagement();
	}

	public void emmenage(){
		// recolle les meubles
		...
		// oublie des affaires
		...
		// fait chier ses parents...
		...
		// et fait chier son proprio à lui faire remplir le dossier de CAF
		// envoie enfin son dossier :
		envoyerDossierCourierANouvelleCAF();
		// oufff... re-glandouille
		this.wait();
	}
}</pre>

Bon mais le problème c'est plus l'implémentation du coté des C.A.F qu'on assimilera à des <a title="Singleton Design Pattern" href="http://fr.wikipedia.org/wiki/Singleton_(patron_de_conception)" target="_blank">Singletons</a> tout simplement parce qu'elles sont uniques et indivisibles par région (enfin tout comme un singleton quoi !) :

Donc bien sûr on a le début suivant :

<pre>import java.math.BigDecimal;

public class CafMoselle implements CAF {

	private static CafMoselle instance = null;
        /**
          * Petit point de technique, les map sont donc des tables de hashages
          * mais la LinkedHashMap est une structure qui contient a la fois une HashMap et une LinkedList
          * ainsi on peut récupérer (si on fait une boucle sur les clés par exemple)
          * les éléments dans l'ordre dans lesquels on les as insérés (ce qui n'est pas le cas avec une HashMap classique)
          *
          */
        private Map&lt;String, String&gt; allocataires = new LinkedHashMap&lt;String, String&gt;();

	private CafMoselle(){
	}

	public static CafMoselle getInstance(){
		if(instance == null)
			instance = new CafMoselle();
		return instance;
	}</pre>

avec plusieurs moyens de discuter avec sa CAF, le téléphone et internet, mais tout d'abord la CAF reçoit le dossier :

<pre>private void recevoirDemandeCourrier(Dossier courrier) throws InterruptedException{
		// exemples de premières synchronizations, pour faire des interruptions volontaires (en ms)
		synchronized(this){
			this.wait(10000); // petite pause de reflexion ...
		}

		System.err.println("humm faudrait faire quelque chose...");

		synchronized(this){
			this.wait(100000); // un peu plus grosse pause de reflexion
		}
		// et si l'étudiant était déjà allocataire ??
		if(!allocataires.containsKey(courrier.getNumeroSecuriteSociale()));
			traiterLeDossier(courrier);
	}</pre>

Le **synchronized** permet ici de mettre un bout de code en tant que section critique, dans le cadre d'une exclusion mutuelle. Ainsi avec le **synchronized** ici, l'objet courant **this**, n'est pas accessible par quelqu'un d'autre pendant toute la durée de l'exécution du code entre accolades.

<span style="color:#ff0000;">Mais attention !!!! Le synchronized n'est pas applicables à des types natifs (int, double, etc...) il n'est applicable qu'a des objets car les objets possèdent des méthodes wait notify/notifyAll, utilisée pour gérer les accès. !!!!</span>

Mais c'est déjà suffisamment d'émotions, pour cet article, donc la suite au prochain épisode, vous saurez comment le dossier sera traité, et les deadlocks/livelock qui peuvent arriver.

++