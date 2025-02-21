---
title: How to debug Django using the Python Debugger PDB
author: ogirardot
type: post
date: 2011-03-15T20:20:39+00:00

original_post_id:
  - 771
categories:
  - Python
tags:
  - Debugger
  - django
  - PDB

---
<p style="text-align:justify;">
  Even if that seems common sense, i found out that there's not that much sources that explains how to use <a title="Python Debugger" href="http://docs.python.org/library/pdb.html" target="_blank">PDB</a> with Django's bundle webserver.  So here we go, let's say you have some treatment like that :
</p>
<!--more-->

<p style="text-align:justify;">
  <pre>def search(request):
	"""
	  	search (it's written up there).
	"""
	if request.method == 'POST':
		item = request.POST['item']
		# separate numeric part from string part and
		# add a % in case no numeric value is provided
	   	(num, test) = re.match("([d]*)([D]*)", item).groups()
		if not num:
			num = "%"
                .... # query in database</pre>

<p>
  Now what we want to check, is that the &#8220;not num&#8221; part is doing its job in replacing any non-numeric part by a wildcard. so we'll add this statement &#8220;import pdb; pdb.set_trace()&#8221; to set up a breakpoint that PDB will be able to use :
</p>

<pre>def search(request):
	"""
	  	ase.
	"""
	if request.method == 'POST':
		item = request.POST['item']
		# separate numeric part from string part and
		# add a % in case no numeric value is provided
	   	(num, test) = re.match("([d]*)([D]*)", item).groups()
                # the breakpoint will be here :
                import pdb; pdb.set_trace()
		if not num:
			num = "%"
                .... # query in database</pre>

<p>
  And then, all we need to do is run the standalone embedded web server of Django using pdb. At first PDB (just like GDB) will wait for you to use the command c (continue) to launch the program and will only stop when he reaches a breakpoint :
</p>

<pre> python -m pdb manage.py runserver</pre>

<p>
  Eventually when you'll reach the breakpoint, you can use the commands locals(), globals() to see all the variables you can access. For more on how to use PDB and debugging in Django i refer you to the nice <a title="PDB by Django" href="https://mike.tig.as/blog/2010/09/14/pdb/" target="_blank">tutorial by Mike Tigas</a>.
</p>

<p>
  <em>Vale</em>
</p>