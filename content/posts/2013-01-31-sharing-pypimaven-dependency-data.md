---
title: Sharing PyPi/Maven dependency data
author: ogirardot
type: post
date: 2013-01-31T10:00:08+00:00

publicize_twitter_user:
  - ogirardot
categories:
  - Data
  - Java
  - OSS
  - Python
tags:
  - dependency graph
  - FTW
  - Sharing

---
<p style="text-align:justify;">
  As time is always running out, i don't think i'll have the time in a while to work again on the data I collected for the last three articles, <a title="Going offline with Maven" href="http://ogirardot.wordpress.com/2013/01/14/going-offline-with-maven/">Going offline with Maven</a>, <a title="State of the maven/java dependency graph" href="http://ogirardot.wordpress.com/2013/01/11/state-of-the-mavenjava-dependency-graph/">State of the Maven/Java dependency graph</a> and <a title="State of the PyPi/Python dependency graph" href="http://ogirardot.wordpress.com/2013/01/05/state-of-the-pythonpypi-dependency-graph/">State of the PyPi/Python dependency graph</a>.
</p>
<!--more-->

<p style="text-align:justify;">
  So, as it took me a long time to build these datasets and even if the datasets were already available on the github project, i want to make it publicly available and define the metadata properly so anyone can reuse them freely. The only licence i'm putting it on is <a href="http://creativecommons.org/licenses/by/2.0/">Creative Commons</a>, so you're free to use it, re-adapt it, publish based on it, or use it for commercial purposes, as long as you mention me (<em>Olivier Girardot <o.girardot (at) lateral-thoughts.com></em>) as author.
</p>

<p style="text-align:justify;">
  So the dataset is divided in three files, compressed using LZMA :
</p>

<h4 style="text-align:justify;">
  <a href="https://github.com/ssaboum/meta-deps/blob/master/mvn-deps.csv.lzma">mvn-deps.csv.lzma</a> and <a id="d70328bf75046f1e732b8048c31ec7f2-1059ead98bb373f5abe26c7027c6f3ceaed1bf30" title="mvn-minimal-deps.csv.lzma" href="https://github.com/ssaboum/meta-deps/blob/master/mvn-minimal-deps.csv.lzma">mvn-minimal-deps.csv.lzma</a>
</h4>

<p style="text-align:justify;">
  <strong>mvn-deps </strong>consists in all the Maven artifacts extracted from Maven central repositories and <strong>mvn-minimal-deps</strong> is the minimal set of dependencies you need to for g<a title="Going offline with Maven" href="http://ogirardot.wordpress.com/2013/01/14/going-offline-with-maven/">oing offline with Maven</a>, once uncompressed both files are a simple <strong>tab-separated csv document</strong> with the following columns :
</p>

<ul style="text-align:justify;">
  <li>
    <span style="line-height:13px;">artifactId</span>
  </li>
  <li>
    groupId
  </li>
  <li>
    version
  </li>
  <li>
    dependencies : <strong>as a base64 encoded json string with the following keys : artifactId, groupId, version </strong>ex: {'artifactId': 'log4j', 'groupId': 'log4j', 'version':'1.0.3&#8242;}
  </li>
</ul>

<h4 style="text-align:justify;">
  <a id="aa84b2f557c8f0e1aa771c617dcbf7f9-187b7a436f0a2a9fb2420b3a72c38372fbdb63c0" title="pypi-deps.csv.lzma" href="https://github.com/ssaboum/meta-deps/blob/master/pypi-deps.csv.lzma">pypi-deps.csv.lzma</a>
</h4>

<p style="text-align:justify;">
  <strong>pypi-deps </strong>consists in all the PyPi dependencies, once again it's a <strong>tab-separated csv document</strong> with the following columns :
</p>

<ul style="text-align:justify;">
  <li>
    name
  </li>
  <li>
    version
  </li>
  <li>
    dependencies : <strong>as a base64 encoded json string with the following keys : name, version </strong>ex: {'artifactId': 'log4j', 'groupId': 'log4j', 'version':'1.0.3&#8242;}
  </li>
</ul>

<p style="text-align:justify;">
  An example on how to treat this file to extract it as a <a title="Networkx" href="http://networkx.github.com/">networkx</a> graph is available in the <a href="https://github.com/ssaboum/meta-deps/blob/master/PyPi%20Metadata.ipynb">github project's IPython notebook</a> that you need to download as a raw file to use it with IPython.
</p>

<p style="text-align:justify;">
  I'd be glad that following <a title="Hilary Mason's blog" href="http://www.hilarymason.com/">Hilary Mason</a> posts on <a title="Sharing data with academics" href="http://www.hilarymason.com/blog/startups-how-to-share-data-with-academics/">sharing data with academics</a> some publications were to use these datasets, if any does, please feel free to comment on this blog post to link to your remixed work.
</p>

<p style="text-align:justify;">
  <em>Vale</em>
</p>