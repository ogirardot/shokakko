---
title: Java Web start/JNLP and file access from within
author: ogirardot
type: post
date: 2009-06-22T20:42:35+00:00

original_post_id:
  - 85
categories:
  - Uncategorized

---
<!--more-->
This post is about an annoying issue that i've been facing with JWS, to sum up java web start is used to deploy throught the web java applications, and execute them as normal applications on a workstation.

Thus you have to deploy it on a Tomcat server (using a war file containing the application itself) and create a jnlp file (Java Network Launching Protocol - an xml based file) to create the environment and specify which class  to launch...

My issue was this one : how can you access the files within the war file and get the last modified date of a precise jar ?

Why ? Because sometimes when you deploy an application on client workstations, you need to copy several files and libraries, to be able to launch more efficiently the application and save configuration/customization files...  
But you don't want those files to be outdated, so you need to check their lastModified timestamp and yours and choose which one should be kept.

So here is a solution that will be explained :

<pre>// to get the URL of the path where we are executing this code :
URL self = MyLaunchedClass.class.getProtectionDomain().getCodeSource().getLocation();

// if you need to get the content of a jar within :
JarInputStream zis = new JarInputStream(MyLaunchedClass.class.getClassLoader().getRessourceAsStream("MyJar.jar"));

for(JarEntry entry = zis.getNextJarEntry(); entry!= null; entry= zis.getNextJarEntry()){

  if(entry.getTime() &gt; maxTime)
    ...
  }
}
</pre>

But what is the most interesting and un-expected thing in the world (well maybe not that much... but whatever) is that if you test something like that :

<pre>URL test = MyLaunchedClass.class.getClassLoader().getRessource("MyJar.jar").toURL();
System.out.println(test);
</pre>

The fact is that you will find (with java 1.6) the internet based URL, while if you test by un-plugging the network connection of your computer at the right time you will find out that there's no outside connection to the network. That's to say that even if the URL given out isn't a local path to the cache stored file, if you try to access this ressource it won't be downloaded once again (which is a good thing).

It's all the more a good thing that in Java 1.5 and before cached files were stored as proper files into the cache directory, but now with Java 1.6 these files are stored assigning them names completely unrelated to the application - almost hashed - and with an _.idx_ file, so there's no way to access these files directly.

This article is maybe a little bit fuzzy, but it's a helping hand that one may need once getting into Java Web Start, to learn more about Java Web Start you can visit this website <http://mindprod.com/jgloss/javawebstart.html> quite complete if you overcome its design.

Äddi everyone.