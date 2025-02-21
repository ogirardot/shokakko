---
title: How to be a happy programmer (with Python) ? 1/3
author: ogirardot
type: post
date: 2011-03-18T21:04:16+00:00

original_post_id:
  - 761
categories:
  - Python
tags:
  - List comprehension
  - Syntax
  - With

---
<p style="text-align:justify;">
  I've just watched <a title="Hillary Mason" href="http://pycon.blip.tv/file/4878710/" target="_blank">Hillary Mason's talk in Pycon 2011</a> : http://pycon.blip.tv/file/4878710/<br /> And that got me thinking about all the python constructs that makes my day better, and i decided to make a list of them and their meaning.
</p>
<!--more-->

<ul style="text-align:justify;">
  <li>
    With
  </li>
</ul>

<p style="text-align:justify;">
  The <em>with </em>keyword is the equivalent of the whole <em>try</em>, <em>catch</em>, <em>finally</em> triplets in Java to handle resources (files, database connections, remote connections, anything that can fail). So the <em>with </em>statement is here to make sure that, for example using a database connection, transaction is started and stopped correctly and can be used as follow :
</p>

[sourcecode language=&#8221;python&#8221;]  
with open('/tmp/my_file', 'w') as p:  
p.write('Writing in a properly closed file resource.')  
[/sourcecode]

<p style="text-align:justify;">
  For a few more example <a title="Python with statement" href="http://effbot.org/zone/python-with-statement.htm" target="_blank">python-with-statement</a>, the official presentation for <a title="Python with statement" href="http://docs.python.org/whatsnew/2.5.html#pep-343-the-with-statement" target="_blank">What's new in 2.5</a> and if you want to know more in order to implement objects usable with the &#8220;with&#8221; statement, you need to see <a title="Python Context Manager" href="http://docs.python.org/library/stdtypes.html#context-manager-types" target="_blank">how the context managers work</a>
</p>

<ul style="text-align:justify;">
  <li>
    List comprehensions
  </li>
</ul>

<p style="text-align:justify;">
  I shouldn't even have to explain how much joy you can gain from using these, especially when you're dealing with object oriented programming or immutable objects, well pretty much anytime you need to operate simple transformation on Lists, dictionnaries, anything iterable, list comprehension is not only the most beautiful way, but also many times, the most efficient way. So here we go for an example :
</p>

[sourcecode language=&#8221;python&#8221;]  
>>> words = 'The quick brown fox jumps over the lazy dog'.split()  
>>> print words  
['The', 'quick', 'brown', 'fox', 'jumps', 'over', 'the', 'lazy', 'dog']  
>>>  
>>> stuff = [[w.upper(), w.lower(), len(w)] for w in words]  
>>> for i in stuff:  
... print i  
...  
['THE', 'the', 3]  
['QUICK', 'quick', 5]  
['BROWN', 'brown', 5]  
['FOX', 'fox', 3]  
['JUMPS', 'jumps', 5]  
['OVER', 'over', 4]  
['THE', 'the', 3]  
['LAZY', 'lazy', 4]  
['DOG', 'dog', 3]  
[/sourcecode]

<p style="text-align:justify;">
  This was the first part of 3 showing  the few Python syntax constructs that makes me &#8220;not&#8221; scream when i try to do things and i don't want to lose time ! See you next time.
</p>

<p style="text-align:justify;">
  <em>Vale</em>
</p>