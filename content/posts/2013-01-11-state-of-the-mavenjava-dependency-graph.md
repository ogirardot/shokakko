---
title: State of the Maven/Java dependency graph
author: ogirardot
type: post
date: 2013-01-11T12:00:28+00:00

publicize_reach:
  - 'a:2:{s:7:"twitter";a:1:{i:2397699;i:350;}s:2:"wp";a:1:{i:0;i:0;}}'
publicize_twitter_user:
  - ogirardot
categories:
  - Data
  - Java
  - OSS
tags:
  - dependency
  - ForceAtlas 2
  - gephi
  - graph
  - maven
  - Spatialization
  - yifan hu

---
<p style="text-align:justify;">
  So here it comes, the second part of a three part articles on dependencies in different world, <a title="State of PyPi dependencies" href="http://ogirardot.wordpress.com/2013/01/05/state-of-the-pythonpypi-dependency-graph/">the first part was about Python/PyPi dependencies</a> and considering the size of the graph : <strong>20661 Nodes, 14047 Edges, </strong> I was able to <a title="PyPi dependencies interactive graph" href="http://ssaboum.github.com/meta-deps">show you the graph in an interactive javascript app</a> using <a title="SigmaJS" href="http://sigmajs.org/">SigmaJS</a>. But this times it's different, after extracting the metadata from <a title="Maven" href="http://maven.apache.org/">Maven</a> repositories, the raw data file generated weights <strong>273M</strong>, and the size of the whole directed dependency graph is <strong>186 384 Nodes and 1 229 083 Edges</strong>, in other words, it's going to be tough to show you the whole graph interactively but the <em>raw data</em>, the <em>graph file</em> and the <em>Gephi file</em> are available on the <a title="MetaDeps" href="http://github.com/ogirardot/meta-deps">GitHub project</a>.
</p>
<!--more-->

<p style="text-align:justify;">
  Handling that much data comes with a cost, that your machine must be prepared to pay... For example, as I tried to export the whole graph into a svg file, Gephi tried to use more than my 16G of RAM and eventually couldn't achieve this. Fortunately, it was no problem to extract it to PNG (and with High Definition), so here comes the pictures.
</p>

<h2 style="text-align:justify;">
  Using Yifan Hu's layout
</h2>

<p style="text-align:justify;">
  As you see the graph is, as expected, pretty dense and this spatialization is not exactly the best one to see what's going on, but I tried it first (even if it's not suitable for large-scale graph processing) in order to compare it with the last article's results. As we can see below, the Java/Python eco-systems are really different in terms of dependency and library usage :
</p>

<figure id="attachment_983" aria-describedby="caption-attachment-983" style="width: 300px" class="wp-caption aligncenter">[<img loading="lazy" decoding="async" class="size-medium wp-image-983 " title="Maven/Java dependency graph using Yifan Hu's layout" src="http://ogirardot.wordpress.com/wp-content/uploads/2013/01/maven-deps-ni-labels.png?w=300" alt="maven-deps-ni-labels" width="300" height="300" />][1]<figcaption id="caption-attachment-983" class="wp-caption-text">Maven/Java dependency graph using Yifan Hu's layout</figcaption></figure>

<figure id="attachment_968" aria-describedby="caption-attachment-968" style="width: 300px" class="wp-caption aligncenter">[<img loading="lazy" decoding="async" class="size-medium wp-image-968 " title="PyPi dependency graph  using Yifan Hu's layout" src="http://ogirardot.wordpress.com/wp-content/uploads/2013/01/pypi-deps.png?w=300" alt="PyPi dependency graph generated using Gephi" width="300" height="300" />][2]<figcaption id="caption-attachment-968" class="wp-caption-text">PyPi dependency graph using Yifan Hu's layout</figcaption></figure>

<p style="text-align:justify;">
  So Yifan Hu's layout is really great for sparsed/simple graphs and even if it took me more than 2 hours to properly compute it with a millin edges, it's worth it just to compare visually the two parts. But now if we want to analyse and get something out of the Maven metadata, let's use a more &#8220;Large Scale Graph&#8221; oriented visualisation. For that we need to choose a parallelizable graph spatialization algorithm.
</p>

<h2 style="text-align:justify;">
  A more suitable approach using ForceAtlas 2
</h2>

<p style="text-align:justify;">
  Force Atlas 2 is a much more suitable algorithm to process large quantity of nodes/edges, firstly because it allowed me to parallelize this computation over 7 CPU, but also because it gives us a clearer overview of what's going on, what is the most popular library and other metrics like that :
</p>

<figure id="attachment_985" aria-describedby="caption-attachment-985" style="width: 640px" class="wp-caption aligncenter">[<img loading="lazy" decoding="async" class="size-large wp-image-985   " src="http://ogirardot.wordpress.com/wp-content/uploads/2013/01/maven-deps-atlas-low.png?w=640" alt="Maven dependency graph" width="640" height="640" />][3]<figcaption id="caption-attachment-985" class="wp-caption-text">Maven dependency graph using Force Atlas 2 (click to see a higher resolution)</figcaption></figure>

<h2 style="text-align:justify;">
  So...
</h2>

<p style="text-align:justify;">
  So now we have the processed data, just at first sight we can see that the Java ecosystem is much more connected than the Python one, there's no judgment here and we will analyse this data in more depth in the next article not to conclude <em>who's the best </em>but more to gain a clearer understanding of a way to go forward for both worlds.
</p>

<p style="text-align:justify;">
  <em>Vale</em>
</p>

 [1]: http://ogirardot.wordpress.com/wp-content/uploads/2013/01/maven-deps-ni-labels.png
 [2]: http://ogirardot.wordpress.com/wp-content/uploads/2013/01/pypi-deps.png
 [3]: http://ogirardot.wordpress.com/wp-content/uploads/2013/01/maven-deps-4096.png