---
title: Compiling OpenOffice.org under MacOs and Linux (Part 1)
author: ogirardot
type: post
date: 2009-02-07T16:46:00+00:00

blogger_blog:
  - readtfb.blogspot.com
blogger_author:
  - ssaboumhttp://www.blogger.com/profile/15320526353188755053noreply@blogger.com
blogger_permalink:
  - /2009/02/compiling-openofficeorg-under-macos-and.html
original_post_id:
  - 9
categories:
  - Uncategorized

---
<!--more-->
I'm currently working on a project with my school (the [Ecole Centrale de Nantes][1]) on OpenOffice.org (OOo) improving the way TabletPC can be used in a Linux environment.  
This project began with some modifications on OpenOffice Impress, so that now you can annotate a slide during the slideshow and change the stroke width and color (you can see the [TabletPC Blog][2](French) for more information).

Therefor we needed to checkout, compile and test OOo and this article is about helping others to get into OOo code and improve it. I personally testes it under two plateforms, MacOs and Linux Ubuntu and i will give you some tips we found with friends of mine.

Some things are pretty common for both plateforms, first of all you need to checkout the svn of OOo for source codes, it's better to use <span style="font-style:italic;">tags </span>rather than the <span style="font-style:italic;">trunk, </span>because it's more stable. For example this is the most recent tag : <span style="font-style:italic;">DEV300_m41</span> so we'll do :

<pre><br />svn checkout http://svn.services.openoffice.org/ooo/tags/DEV300_m41<br /></pre>

The svn is quite large, so it will obviously take some time, so relax and prepare the next steps. Different plateforms have different needs :

  * MacOs :

This script was created by Jonathan Winandy and Pierre-jean Parot, with the help of Eric Bachard and as a user i'm presenting this to you.  
So you'll need as a first thing [XCode][3](gives a c++ compiler to MacOs) and [Fink][4](gives apt-get), and when these two things are installed you can start by getting the <span style="font-style:italic;">wget </span>command typing into the terminal :

<pre><br />sudo apt-get install wget<br /></pre>

then you go into the directory where you did put OpenOffice's checkout and use these lines, one by one :

<pre><br />wget http://tools.openoffice.org/unowinreg_prebuild/680/unowinreg.dll<br />mv unowinreg.dll external/unowinreg/<br />wget http://eric.bachard.free.fr/mac/moz/seamonkey_Intel/MACOSXGCCIinc.zip<br />wget http://eric.bachard.free.fr/mac/moz/seamonkey_Intel/MACOSXGCCIlib.zip<br />wget http://eric.bachard.free.fr/mac/moz/seamonkey_Intel/MACOSXGCCIruntime.zip<br />mv MACOSXGCCIinc.zip moz/zipped/<br />mv MACOSXGCCIlib.zip moz/zipped/<br />mv MACOSXGCCIruntime.zip moz/zipped/<br />wget http://eric.bachard.free.fr/mac/aquavcl/patches/<br />aqua_November_2008/26th_november/moz2seamonkey_connectivity.diff<br />patch --dry-run -p0 &lt; moz2seamonkey_connectivity.diff<br />patch -p0 &lt; moz2seamonkey_connectivity.diff</pre>

This is using the website of Eric Bachard, leading the port of OOo under MacOs, and once you've got these lines working, you can start to configure your build once and for all:

<pre><br />export PATH="/usr/bin:/bin:/usr/sbin:/sbin:/usr/local/bin:/usr/texbin:<br />/usr/X11/bin:/usr/X11R6/bin:/opt/local/bin:/opt/local/sbin:/sw/bin:<br />/sw/sbin:/Library/Frameworks/Python.framework/Versions/Current/bin"<br /><br />./configure --without-nas --disable-cups --disable-pasf --disable-crashdump<br />--disable-freetype --disable-fontconfig --with-epm=internal<br />--with-jdk-home=/System/Library/Frameworks/JavaVM.framework/Home<br />--disable-gtk --disable-gnome-vfs --with-system-curl --with-stlport=no<br />--disable-build-mozilla --disable-mediawiki<br />--enable-werror --enable-presenter-screen --with-use-shell=bash<br /><br />./bootstrap<br /></pre>

Then if everything went okay, you just need to add some variables with <span style="font-style:italic;">export </span>and add the build tools to your terminal :

<pre><br />export CC="ccache gcc"<br />export CXX="ccache g++"<br />export MACOSX_DEPLOYMENT_TARGET=10.5<br />export VERBOSE=TRUE<br /><br />source MacOSXX86Env.Set.sh<br /></pre>

Then at last you can start to build, with these lines, after this first build is finished (about 8-9 hours on MacBook Air), you will find a dmg to mount OpenOffice.org 3, the one you built.

<pre><br />cd instsetoo_native/<br />nice -10 perl ../solenv/bin/build.pl --all -P4<br /></pre>

You can rebuild it after modification, using the last steps again, and it will take much less time, about 22 minutes on a MacBook Air. I will give in a future article a better way to deal with tests on OOo with MacOs, stay tuned.

 [1]: http://www.ec-nantes.fr/
 [2]: https://pedagogie.ec-nantes.fr/tablet-pc
 [3]: http://developer.apple.com/TOOLS/xcode/
 [4]: http://www.finkproject.org/