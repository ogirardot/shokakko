---
title: Advanced use of Eclipse for Java
author: ogirardot
type: post
date: 2009-10-27T07:00:26+00:00

original_post_id:
  - 259
categories:
  - Java
tags:
  - dev
  - Eclipse
  - learning
  - Programming
  - Software Development
  - tricks
  - Useful Tips

---
<!--more-->
Eclipse is a wonderful tool when you're developing big projects using the Java programming language, but it's easy to just see the monster as one big, buggy, slow, heck of a program and just enjoy a simple text editor with a few tweakings for syntax highlighting.

But as you come to use it on a regular basis a few tricks comes, almost naturally, to help you get more efficient. I'm just going to present a few that i use regularly, and that makes me keep using Eclipse :

  * **Auto-completion tool (+ with javadoc) : Ctrl + space**

I won't go presenting this one, everyone knows, but you just have to test it to be able to savour it the way it should.

  * **Open type (class, Interface etc...) search engine : Ctrl + shift + T (Cmd + shift + T under MacOsX)**

<p style="text-align:center;">
  <a rel="attachment wp-att-262" href="http://www.readtfb.net/2009/10/27/advanced-use-of-eclipse-for-java/opentype/"><img loading="lazy" decoding="async" class="aligncenter size-full wp-image-262" title="openType" src="http://ogirardot.wordpress.com/wp-content/uploads/2009/10/opentype.png" alt="openType" width="425" height="354" /></a>
</p>

  * **Quick refactor : Alt + shift + R**

<p style="text-align:center;">
  <a rel="attachment wp-att-263" href="http://www.readtfb.net/2009/10/27/advanced-use-of-eclipse-for-java/refactor/"><img loading="lazy" decoding="async" class="aligncenter size-full wp-image-263" title="refactor" src="http://ogirardot.wordpress.com/wp-content/uploads/2009/10/refactor.png" alt="refactor" width="537" height="156" /></a><strong> </strong>
</p>

  *  **Delete a line : ctrl + D**

  * **Delete a line, word by word (with camelCase handling)**

Allows you to delete Words step by step : _e.g. parRotSubscription_ will become  _parRot_ 

  * **Change method Signature  : Alt + shift + C**

<a rel="attachment wp-att-272" href="http://www.readtfb.net/2009/10/27/advanced-use-of-eclipse-for-java/signature/"><img loading="lazy" decoding="async" class="aligncenter size-medium wp-image-272" title="signature" src="http://ogirardot.wordpress.com/wp-content/uploads/2009/10/signature.png?w=265&#038;h=300" alt="signature" width="265" height="300" /></a>

  * **Usage search of a method/attribute : Ctrl + Shift + G**

You just have to select the attribute or method, and it will search for you where this object is used.

  * **Show the type hierarchy : F4**

Shows the full hierarchy of the class/interface, showing ancestors and descendents.

<a rel="attachment wp-att-285" href="http://www.readtfb.net/2009/10/27/advanced-use-of-eclipse-for-java/call/"><img loading="lazy" decoding="async" class="aligncenter size-medium wp-image-285" title="call" src="http://ogirardot.wordpress.com/wp-content/uploads/2009/10/call.png?w=300&#038;h=220" alt="call" width="300" height="220" /></a>

  * **Organize Imports : Ctrl + Shift + O**

This is, in my view, the most useful function : it allows you to remove un-used imports (e.g. **_import java.io.IOException;_**) and add the one you need &#8220;precisely&#8221; so that you don't go adding **import java.***

  * **Comment selected lines : Ctrl + shift + C**

The last one to comment full paragraphs and lines of code, and un-comment them when you need to.

I know that many of so-called geeks are able to make vim or emacs do it all, but the refactor functions of the jdt Core of Eclipse makes it so easy, it's just a burden sometimes to have to use other IDE.