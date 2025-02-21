---
title: Accentuate, KeyJnote under MacOs
author: ogirardot
type: post
date: 2009-02-04T12:18:00+00:00

blogger_blog:
  - readtfb.blogspot.com
blogger_author:
  - ssaboumhttp://www.blogger.com/profile/15320526353188755053noreply@blogger.com
blogger_permalink:
  - /2009/02/accentuate-keyjnote-under-macos.html
original_post_id:
  - 8
categories:
  - Uncategorized

---
<!--more-->
I had, and friends of mine too, some issues getting the new KeyJNote work under MacOs. For those who don't know or remember KeyJNote was a simple program designed to create great transitions for pdf (Beamer based) slideshows.  
Then Apple decided the name of this OpenSource program was to close to its product KeyNote,  
and asked for hara-kiri.

The program was therefor removed for the internet and reborned into two programs, one of them is Accentuate and the other is [Impressive][1], but none of them is really easy to use with MacOSX, because they need some packages with Python that are not included in the OS.

The good way of achieving it, is to install the dependencies via [MacPorts][2], and with the following command lines :

<pre><br />sudo port install py25-opengl py25-game xpdf ghostscript pdftk<br /></pre>

But as a matter of fact port is not exactly dealing with binaries, so this command can last a while, because it compiles these packages (<span style="font-style:italic;">ghostscript</span> is not compulsory but it can be usefull).

Then you just need to use the command line <span style="font-style:italic;">python</span> <span style="font-style:italic;">accentuate</span> <span style="font-style:italic;"></span> to make it work,  
one point is particulary relevant is that Accentuate takes full support of the Apple Remote.

I hope this article will help because it took me several hours to find out.

 [1]: http://www.google.com/url?sa=t&source=web&ct=res&cd=4&url=http%3A%2F%2Fwww.silvyn.net%2Fblog%2Findex.php%3Fpost%2F2008%2F12%2F08%2FkeyJnote-change-de-nom-pour-Impressive&ei=JKWJSajsLomR_gb_vf3QBw&usg=AFQjCNEXgDrjZoqh4PfcCDCR6m8ShhLptg&sig2=HLzEgHjMbhCJb8zJHwQLdw
 [2]: http://www.macports.org/