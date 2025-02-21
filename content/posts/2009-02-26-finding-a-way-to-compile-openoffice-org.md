---
title: Finding a way to compile OpenOffice.org
author: ogirardot
type: post
date: 2009-02-26T18:17:00+00:00

blogger_blog:
  - readtfb.blogspot.com
blogger_author:
  - ssaboumhttp://www.blogger.com/profile/15320526353188755053noreply@blogger.com
blogger_permalink:
  - /2009/02/finding-way-to-compile-openofficeorg.html
original_post_id:
  - 11
categories:
  - Uncategorized

---
<!--more-->
The last articles were a helping to getting into this project, but now it's time to find a better way to deal with changed into the code of OOo.

When you compile, as the last article describe it, you're actually compiling the project as a whole and re-creating the installed files (or the dmg file), but there's more than this to do.

Most of the time, one only change parts of the files in a module, let's say <span style="font-style:italic;">slideshow </span>that is dealing with the slideshow in OpenOffice Impress, if you put yourself into this module, you can build it using the <span style="font-style:italic;">build </span>command and get the compiled files into the <span style="font-style:italic;">unx******/</span> directory.

The skillfull way is then to copy just the files you need, just the library files that are produced by this module.

Under MacOs, you need to install the dmg files into the Applications folder, and then you can copy the files using this command line :

<pre><br />cp -vf unxmacxi.pro/lib/* /Applications/OpenOffice.org.app/Contents/<br />basis-link/program/<br /></pre>

Under Linux, if you did put that you wanted &#8220;installed files&#8221;, you just need to change the path to where <span style="font-style:italic;">instsettoo_native</span> put these files.

With this method, you reduce the compiling time from 20 minutes to a few seconds and you can therefor speed up the pace and test things more easily.

I hope these articles on OOo will be helpfull, see you next time.