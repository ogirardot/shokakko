---
title: 'Useful Tips : How to insert BLOB into an Oracle database using JDBC'
author: ogirardot
type: post
date: 2009-06-05T17:19:00+00:00

blogger_blog:
  - readtfb.blogspot.com
blogger_author:
  - ssaboumhttp://www.blogger.com/profile/15320526353188755053noreply@blogger.com
blogger_permalink:
  - /2009/06/useful-tips-how-to-insert-blob-into.html
original_post_id:
  - 29
categories:
  - Uncategorized

---
<!--more-->
It can be quite disturbing to try to insert a BLOB (Binary Large OBject) using JDBC, because most of the time java softwares will handle BLOB, not exactly using their own time, but using Strings.  
And if you try let's say this kind of query :

<pre><br />// blob String :<br />String blobToInsert = ....;<br /><br />StringBuilder insertQuery = new StringBuilder();<br />insertQuery.append("insert into ");<br />insertQuery.append(" table( table_id, table_blob_field ) ");<br />insertQuery.append("values ");<br />insertQuery.append(" ('64', '" + blobToInsert + "' )");<br /><br /></pre>

And try to execute it,you will end up having several different kind of errors, once Oracle may tell you that the field is too long, to finally end up telling you a <span style="font-style:italic;">strange</span> type error.

The trick is the following, you can't use Strings (or StringBuilder/StringBuffer/etc... ) because it won't define the type of Object you'll be using. So you'll have to use a prepare statement (normally used for optimization when you're using more than once a query) this way :

<pre><br />// blob String :<br />String blobToInsert = ....;<br /><br />StringBuilder insertQuery = new StringBuilder();<br />insertQuery.append("insert into ");<br />insertQuery.append(" table( table_id, table_blob_field ) ");<br />insertQuery.append("values ");<br />insertQuery.append(" ('64', ? )");<br />PrepareStatement psInsert = con.prepareStatement(insertQuery.toString());<br /><br />// and then set :<br />psInsert.setBytes(1, blobToInsert.getBytes());<br /><br />// and execute the query :<br />psInsert.execute();<br /></pre>

This way you won't be bothered, because nowadays JDBC connection using Oracle drivers can recognize BLOBs and do the work for you.  
Enjoy.