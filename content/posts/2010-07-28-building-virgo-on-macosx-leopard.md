---
title: Building Virgo on MacOsX Leopard
author: ogirardot
type: post
date: 2010-07-28T20:50:32+00:00

original_post_id:
  - 408
categories:
  - Java
tags:
  - build
  - git
  - jee6
  - springsource
  - virgo

---
I won't say that my contribution to this work will be tremendous, but it's nice to know a little more if you want to try out building the ex SpringSource DM Server
<!--more-->
A lot is already available using the Virgo project documentation : <a title="Virgo Build" href="http://wiki.eclipse.org/Virgo/Build" target="_blank">http://wiki.eclipse.org/Virgo/Build</a>

But to sum it up, you just need to install <a title="Git Download" href="http://git-scm.com/download" target="_blank">Git</a>, and clone the existing repositories available at <a title="Virgo source repo" href="http://wiki.eclipse.org/Virgo/Source" target="_blank">http://wiki.eclipse.org/Virgo/Source</a>. The little tweakings you'll need are more about MacOs's profound attachment to Java 1.5 and the Virgo's clear intention to move-on to (at least) Java 1.6 🙂

So to change your default Java 1.5 JDK Configuration you've got the perfect utility tool bundled within Mac's twisted flesh, file:///Applications/Utilities/Java Preferences.app

Just drag and drop Java 6 to the top and you'll be just fine.

Next step is to make Ant understand your intention to move on as well, just export the JAVA_HOME env variable to the right value :

<div id="_mcePaste" style="text-align:center;">
  export JAVA_HOME=/System/Library/Frameworks/JavaVM.framework/Versions/1.6/Home
</div>

And once again you'll be just fine.

Enjoy