---
title: State of the Python/PyPi dependency graph
author: ogirardot
type: post
date: 2013-01-05T19:28:39+00:00

publicize_twitter_user:
  - ogirardot
twitter_cards_summary_img_size:
  - 'a:6:{i:0;i:1024;i:1;i:1024;i:2;i:3;i:3;s:26:"width="1024" height="1024"";s:4:"bits";i:8;s:4:"mime";s:9:"image/png";}'
categories:
  - Data
  - Python
tags:
  - data
  - dll hell
  - gephi
  - pip

---
<p style="text-align:justify;">
  I usually work in Java/Maven environment, so when I explain to people that Python also has a package manager - a bit less heavy than maven - and that it's working pretty well, I always have to answer the same question : &#8220;Ok, but <strong>how does it solve the transitive dependency hell</strong> ?&#8221;
</p>
<!--more-->

<p style="text-align:justify;">
  Also known as the historic <em>DLL Hell</em>/<em>Jar Hell</em> etc... In short, when you depend on A and C, that A depends on B (version 1.2) and C depends on B (version 1.5) :  <strong>How do you choose which version of B you will take ?</strong>
</p>

<p style="text-align:justify;">
  I ended up trying to answer, not exactly that question, but why I never really had that problem in Python. So this article is the first of a three part series you could call &#8220;<strong>Dependency as a liability</strong>&#8220;.
</p>

<p style="text-align:justify;">
  In this part, I wanted to analyse the Python library world in terms of a full dependency graph - how every library depends on each other.
</p>

<p style="text-align:justify;">
  After talking with <a title="Fetchez le Python" href="http://ziade.org/">Tarek Ziadé</a> about that, he told me how complicated things are right now. It seems that, for now, the way things are, the only complete and secure way to know what a package needs in terms of dependency is to execute its installation on every operating system. This was a bit out of my scope for now, so I took another way, just to see where it would lead me.
</p>

<h2 style="text-align:justify;">
  Analyzing setup.py files
</h2>

<p style="text-align:justify;">
  For recent packages, following <a title="H2G2" href="http://guide.python-distribute.org/">the Hitchiker's Guide to packaging</a>, the metadata of the package are stored in file called <strong>setup.py </strong> that looks like this :
</p>

[sourcecode language=&#8221;python&#8221;]  
from distutils.core import setup

setup(  
name='TowelStuff',  
version='0.1.0&#8242;,  
author='J. Random Hacker',  
author_email='jrh@example.com',  
packages=['towelstuff', 'towelstuff.test'],  
scripts=['bin/stowe-towels.py','bin/wash-towels.py'],  
url='http://pypi.python.org/pypi/TowelStuff/',  
license='LICENSE.txt',  
description='Useful towel-related stuff.',  
long_description=open('README.txt').read(),  
install_requires=[  
&quot;Django >= 1.1.1&quot;,  
&quot;caldav == 0.1.4&quot;,  
],  
)  
[/sourcecode]

You can notice a few things like the _author, version, author_email, url, license... _and what I was focusing on the _install_requires _parameter, where you declare all your dependencies. the problem is, that it may sound simple, but the _setup.py _file is a python script in itself, so the _install_requires _directive can be changed when the script is executed.

So I took my chances, and decided to create a project to extract dependencies from all packages on PyPi according to the _install_requires _parameter and see if this is **mainly used statically or dynamically.** So what the **[meta-deps][1] **project does is :

  * extract all packages from PyPi using the XML-RPC api;
  * download the releases and extract from the _setup.py _file the _install_requires _dependency;
  * Store the results in a csv file **pypi-deps.csv**;

If you want to re-use the raw data, you don't need to re-execute the process (and overload PyPi servers in the meantime), just download the [**pypi-deps.csv**][2] file, it contains just these columns :

  * <span style="line-height:13px;">name of the dependency</span>
  * version extracted
  * a base64 encoded, json string to store the list of dependencies : so you just need to execute **json.loads(b64decode(...))**

## Results

So what comes out of all this ? This graph :

<figure id="attachment_968" aria-describedby="caption-attachment-968" style="width: 640px" class="wp-caption aligncenter">[<img loading="lazy" decoding="async" class="wp-image-968 size-large" src="https://ogirardot.wordpress.com/wp-content/uploads/2013/01/pypi-deps.png?w=640" alt="PyPi dependency graph generated using Gephi" width="640" height="640" />][3]<figcaption id="caption-attachment-968" class="wp-caption-text">PyPi dependency graph - click to see the interactive version</figcaption></figure>

Ok, if you see it like that, you must think it looks like a huge jellyfish, and that i'm just joking with you. So I spent a little time creating and optimizing [an interactive graph of the PyPi dependency][4] (it seems to be best to open it using chrome) where you can scroll and see all the dependencies with all the metrics and explanation needed.

The next steps will be to do the same with Maven dependencies in a Java world, and compute metrics needed to compare the both.

_Vale_

 [1]: https://github.com/ogirardot/meta-deps "meta-deps"
 [2]: https://github.com/ogirardot/meta-deps/blob/master/pypi-deps.csv.lzma "PyPi dependency data"
 [3]: http://ogirardot.github.com/meta-deps/
 [4]: http://ogirardot.github.com/meta-deps/ "Interactive graph of the PyPi dependency"