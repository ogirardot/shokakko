---
title: Comment créer des sous-formulaires à partir d’un Model Django
author: ogirardot
type: post
date: 2010-09-09T10:00:53+00:00

original_post_id:
  - 497
categories:
  - Python
tags:
  - CRUD
  - django
  - Form

---
<!--more-->
<p style="text-align:justify;">
  Un petit article Python de temps en temps ne fait pas de mal, donc ici je vais détailler la manière  <strong>très simple</strong> d'utiliser les modèles Django pour créer des formulaires, mais aussi créer des sous-types de formulaire à partir du même modèle. Tout va s'expliquer :
</p>

<p style="text-align:justify;">
  Quand on créé un objet avec la couche de persistance de Django, on crée une nouvelle classe :
</p>

<pre>class User(models.Model):
	username = models.CharField(max_length=255)
	password = models.CharField(max_length=255)
	email = models.EmailField(max_length=255)
	blog = models.URLField(verify_exists=False)
	created_at = models.DateTimeField(auto_now=True, auto_now_add=True)

	def __unicode__(self):
		return self.username</pre>

<p style="text-align:justify;">
  Cette classe permet de gérer automatiquement les données base de données, la DAO, et quelques validations. Maintenant il n'est pas rare de vouloir lier un formulaire à un tel objet, pour gérer de simple CRUD. Rien n'est plus simple avec Django où il suffit d'écrire le code suivant (toujours dans <em>models.py</em>) :
</p>

<pre>class UserForm(forms.ModelForm):
	class Meta:
		model = User</pre>

<p style="text-align:justify;">
  Ce code <em>complexe </em>permet non-seulement de gérer les champs associés à l'objet <em>User</em>, mais il possède aussi une interaction facilitée avec tous les objets persistants, voilà quelques exemples d'utilisations :
</p>

<pre># loading object
	aUser = User.objects.get(id=id);
	# using it to create a bounded form to this object
	aUserForm = UserForm(instance=aUser)</pre>

<p style="text-align:justify;">
  On peut aussi récupérer une instance de formulaire à partir d'un <em>submit </em>en utilisant la commande :
</p>

<pre style="text-align:justify;">form = UserForm(request.POST)
    if form.is_valid():
         #etc.</pre>

<p style="text-align:justify;">
  Cette validation permet d'obtenir un objet <em>clean</em> avant la sauvegarde. Maintenant même si tout cela est très intéressant, ces informations sont présentes de bases dans la documentation officielle de Django, et je vais plutôt approfondir un point. <strong>Comment créer un formulaire à partir d'un modèle qui n'affiche pas tous les champs du modèle ?</strong>
</p>

<p style="text-align:justify;">
  Le système est prévu aussi pour ça, il permet de créer des sous-formulaires en sélectionnant les champs à afficher, on peut donc créer deux formulaires différents, une simple et une avancée :
</p>

<pre>class SimpleUserForm(forms.ModelForm):
	class Meta:
		model = User
		fields = ('username', 'email', 'blog')

class AdvancedUserForm(forms.ModelForm):
	class Meta:
		model = User
		fields =  ('username', 'password', 'email', 'blog')</pre>

<p style="text-align:justify;">
  Ces formulaires partiels peuvent ainsi servir à afficher l'objet ou permettre l'édition mais que sur certains champs.
</p>

<p style="text-align:justify;">
  <em>Vale</em>
</p>