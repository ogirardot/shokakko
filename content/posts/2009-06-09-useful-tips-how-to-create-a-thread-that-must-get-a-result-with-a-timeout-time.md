---
title: 'Useful Tips : How to create a thread that must get a result with a timeout time'
author: ogirardot
type: post
date: 2009-06-09T16:41:00+00:00

blogger_blog:
  - readtfb.blogspot.com
blogger_author:
  - ssaboumhttp://www.blogger.com/profile/15320526353188755053noreply@blogger.com
blogger_permalink:
  - /2009/06/useful-tips-how-to-create-thread-that.html
original_post_id:
  - 31
categories:
  - Uncategorized

---
<!--more-->
Many times threads are really useful to use, but most of the time you don't want an asynchronous thread to run forever... Especially when you want it to execute tasks or fetch some data from a servlet...  
It's not always easy to think about how to do it, i once used another Timer thread to wake up this one, and it's what one should do for regular tasks and/or real time systems, but for a one time task here's the best solution i've ever used :



<pre><br />  Callable&lt;Integer&gt; call = new Callable&lt;Integer&gt;() {<br />  public Integer call() throws Exception {<br />        // let's say you have here some task that will return an Integer value<br />        Integer result = ...;<br />        return result;<br />    }<br />  };<br /><br />  ExecutorService service = Executors.newSingleThreadExecutor();<br />  try {<br />    Future&lt;Integer&gt; ft = service.submit(call);<br />    try {<br />  <br />      // the concept of future Integer will allow you to get it using a timeout<br />      // here we precise that the task must take less than 3 minutes :<br />      int exitVal = ft.get(180, TimeUnit.SECONDS);<br />      return exitVal;<br />  <br />    } catch (TimeoutException to) {<br />  <br />      System.err.println("The server took too long to answer, timeout reached !");<br />  <br />    } <br /><br />  } finally {<br />    service.shutdown();<br />  }<br /><br /><br /></pre>

Therefor, to sum up, you must encapsulate your task in a Callable object, and execute it using the Executors class. It's the simplest way, and you don't deal with the whole lifecycle of the working Thread.  
Enjoy.

P.S. correcting the code because the executor service must be shutdown after use