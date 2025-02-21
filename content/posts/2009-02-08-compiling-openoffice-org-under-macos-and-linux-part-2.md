---
title: Compiling OpenOffice.org under MacOs and Linux (Part 2)
author: ogirardot
type: post
date: 2009-02-08T14:52:00+00:00

blogger_blog:
  - readtfb.blogspot.com
blogger_author:
  - ssaboumhttp://www.blogger.com/profile/15320526353188755053noreply@blogger.com
blogger_permalink:
  - /2009/02/compiling-openofficeorg-under-macos-and_08.html
original_post_id:
  - 10
categories:
  - Uncategorized

---
<!--more-->
In the last article we saw mainly how to compile under MacOs where the process is quite simple, it is actually even simpler under Linux, but you need to fulfil the right dependancies when configuring your system.

  * Linux :

So under linux, when you've your correct checkout of the svn, you need just like under MacOs to run the <span style="font-style:italic;">./configure </span>file and i can give you just one advice that is quite crucial <span style="font-weight:bold;">never use the OpenJDK for Java </span>if you want to compile OOo one day always choose the Sun Java JDK.

You'll also need to download the mozilla sources like you did on MacOs, the configure file gives you the link when it needs it, so no problem.

When you run the configure file under <span style="font-style:italic;">*nux </span>you will be asked step by step to fulfill several dependancies that are needed in order to compile properly (mainly <span style="font-style:italic;">-dev </span>packages), and you'll have to choose the compiler, you must do so by typing the following line when launching the configure file :

<pre><br />./configure --with-mingwin=i586-mingw32msvc-g++<br /></pre>

And now if nothing failed you'll have to run just like under MacOs the following lines :

<pre><br />./bootstrap<br />source LinuxX86Env.Set.sh<br /></pre>

And finally start the real build by going into the directory <span style="font-style:italic;">instsetoo_native</span> and typing :

<pre><br />build --all<br /></pre>

It should take a while but then you'll have by default <span style="font-style:italic;">.deb </span>files under the <span style="font-style:italic;">unx**** </span>directory, and if you want the install files that you can run directly you just need to re-build the whole system and change the variable PKGFORMAT before by typing :

<pre><br />export PKGFORMAT="installed"<br /></pre>

on the next post we'll see how to improve the way one can build OOo in terms of compiling time, because you can't do real testing when you need to wait 22 minutes before seeing the results of each tests.  
You can also see more on the website of OOo : [Building OpenOffice.org][1] and once again, don't hesitate to post comment if you're getting trouble.

 [1]: http://wiki.services.openoffice.org/wiki/Building_OpenOffice.org