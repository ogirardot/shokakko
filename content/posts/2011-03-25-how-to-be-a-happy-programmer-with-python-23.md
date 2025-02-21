---
title: How to be a happy programmer (with Python) ? 2/3
author: ogirardot
type: post
date: 2011-03-25T21:41:44+00:00

original_post_id:
  - 787
categories:
  - Python
tags:
  - happiness
  - Happiness is a state of mind

---
<p style="text-align:justify;">
  In the series of the Python &#8220;features&#8221; that makes me happy <a title="how-to-be-a-happy-programmer-with-python" href="http://www.readtfb.net/2011/03/18/how-to-be-a-happy-programmer-with-python-13/" target="_blank">last time</a> i began with two concepts, the <em>with</em> statement and the <em>list comprehensions</em>, now i'm going to talk about Multiple assignments and the import aliases.
</p>
<!--more-->

<div style="text-align:justify;">
  <ul>
    <li>
      Multiple assignments
    </li>
  </ul>
</div>

<p style="text-align:justify;">
  It's a simple idea that lets you return a series of value and on the other end assign those multiple values at the same time, example when you're splitting a string or extracting groups from a regular expression :
</p>

[sourcecode language=&#8221;python&#8221;]  
>>> split_me ="here,we, are,again"  
\# splitting we'll get a list of values  
>>> split_me.split(",")  
['here', 'we', ' are', 'again']  
\# if you don't know the number of values you're going to have  
\# you can't use this features, example :  
>>> (start,end) = split_me.split(",")  
Traceback (most recent call last):  
File "", line 1, in  
ValueError: too many values to unpack  
\# but if you know that there's going to be n values :  
>>> (start,end) = split_me.split(" ")  
>>> start  
'here,we,'  
>>> end  
'are,again'  
[/sourcecode]

<div style="text-align:justify;">
  <ul>
    <li>
      Import aliases
    </li>
  </ul>
</div>

<p style="text-align:justify;">
  It means what it says, when you import a library or module, you can use aliases, example in Django for shortcuts :
</p>

[sourcecode language=&#8221;python&#8221;]  
\# this is extracted from my own code :  
from django.http import HttpResponse, HttpResponseRedirect as redirect  
from django.shortcuts import render\_to\_response as render</pre>  
[/sourcecode]

<p style="text-align:justify;">
  I won't start to talk about the standard library as a whole, or libs like Numpy, Scypy, scikit-learn, .... that makes it so easy to just think in Python.
</p>

<p style="text-align:justify;">
  And you ? What are the Python constructs (2.x or 3.x) that makes you feel happy and efficient at the end of the day ?
</p>