---
title: Sur le site d’une grande banque…
author: ogirardot
type: post
date: 2009-12-09T05:30:37+00:00

original_post_id:
  - 375
categories:
  - Uncategorized

---
voilà ce qu'on pourrait trouver, alors qu'on essaye sans succès sur une page Web de changer son mot de passe :
<!--more-->
<pre>&lt;script language="javascript"&gt;
function ifEnterIsPressed() {
	if (window.event.keyCode==13) {
		submitTheForm();
	}
}

function submitTheForm() {
	if ( (document.theform.password0.value.length &gt;= 10) && (document.theform.password1.value.length &gt;= 10) && (document.theform.password2.value.length &gt;= 10) && (document.theform.password0.value.length &lt;= 20) && (document.theform.password1.value.length &lt;= 20) && (document.theform.password2.value.length &lt;= 20) ) {
		document.theform.submit();
	}
	else {
		alert("Au moins un champs obligatoire est vide ou erroné.");
	}
}

&lt;/script&gt;

</pre>

Dans le style message d'erreur super-pas clair, où sans regarder le code j'aurai pas réussi à valider mon nouveau mot de passe, je trouve ça pas mal...