---
title: 'Useful Tips : How to find the biggest files on a unix system ?'
author: ogirardot
type: post
date: 2009-06-03T10:35:00+00:00

blogger_blog:
  - readtfb.blogspot.com
blogger_author:
  - ssaboumhttp://www.blogger.com/profile/15320526353188755053noreply@blogger.com
blogger_permalink:
  - /2009/06/useful-tips-how-to-find-biggest-files.html
original_post_id:
  - 28
categories:
  - Uncategorized

---
<!--more-->
Useful mainly when you've got a problem on a unix system because of the space disk. Because some still manage to fill a server with logs or temporary files especially when some programs go nuts and loop without end.

So whenever this happens, and you're in a directory that contains part of the problem, you can type that :



<pre><br />find . -size +50000 -exec ls {} ; | sort -nr | more<br /></pre>

Then, with this command line, you'll be able to find files that are larger than 50 000 blocks long, and they will be sorted and paginated by <span style="font-style:italic;">more</span>.  
Enjoy !