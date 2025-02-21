---
title: 'Useful Tips : StringBuilder and Java String Concatenation'
author: ogirardot
type: post
date: 2009-11-14T05:30:22+00:00

original_post_id:
  - 336
categories:
  - Java

---
<!--more-->
As you certainly now, string concatenation in Java can be achieved using the &#8220;+&#8221; operator like that :

<pre>String parText = "This " + "is " + "a full text";
parText = parText + " in a new String object";
</pre>

<p style="text-align:justify;">
  But each time you try to concat the string in a new line a new <strong>String </strong>object is created, therefor the String concatenation may end up as being a performance issue.
</p>

<p style="text-align:justify;">
  To fix this we use two types of objects : <em><strong>StringBuilder </strong></em>and <em><strong>StringBuffer</strong></em>. Then you can translate your String (e.g. a SQL Query) into :
</p>

<pre>StringBuilder stb = new StringBuilder();
stb.append("This ");
stb.append("is ");
stb.append("a full text");
stb.append(" in a new String object");
</pre>

The main differences between StringBuilder and StringBuffer is that the last one is **synchronized** and may be used in a multi-threaded context while the other one won't have any type of synchronization. Luckily when you try to concatenate multiple string items, Eclipse IDE, (if you ask it by typing **Ctrl + 1**), will ask you if you need to translate it to a StringBuilder object :

<p style="text-align:center;">
  <a rel="attachment wp-att-337" href="http://www.readtfb.net/2009/11/14/useful-tips-stringbuilder-and-java-string-concatenation/stb/"><img loading="lazy" decoding="async" class="aligncenter size-full wp-image-337" title="StringBuilder" src="http://ogirardot.wordpress.com/wp-content/uploads/2009/11/stb.png" alt="StringBuilder" width="572" height="203" /></a>
</p>

<p style="text-align:justify;">
  But nowadays, if you're careful and know what you're doing, you can avoid typing this kind of <em>&#8220;ultra verbose&#8221;</em> way of dealing with strings. Actually since Java 1.5 String concatenations when they're done on a whole line, are directly translated by Java Compiler, but if you jump onto another line and keep on concatenating the same String, optimization will be lost. E.g. :
</p>

<p style="text-align:left;">
  <pre>
// this way
StringBuilder stb = new StringBuilder();
stb.append("This ");
stb.append("is ");
stb.append("a full text");
stb.append(" in a new String object");
// and this one are equivalent
String parText = "This " + "is " + "a full text" + " in a new String object";

// but this one isn't :
String parText2 = "This " + "is " + "a full text";
parText2 = parText2 + " in a new String object";
</pre>

<p style="text-align:left;">