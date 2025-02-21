---
title: Going offline with Maven
author: ogirardot
type: post
date: 2013-01-14T19:16:41+00:00

publicize_twitter_user:
  - ogirardot
publicize_reach:
  - 'a:2:{s:7:"twitter";a:1:{i:2397699;i:352;}s:2:"wp";a:1:{i:0;i:0;}}'
categories:
  - Data
  - Java
  - NoSSII
tags:
  - dependencies
  - maven
  - minimal
  - offline

---
<p style="text-align:justify;">
  At <a title="LT" href="http://www.lateral-thoughts.com">Lateral-Thoughts</a>, we organize at least once a year, what we call a &#8220;Timeoff&#8221; where we get together in a nice place and hack on what we want. It can be a learning period or a <a title="Startup Weekend" href="http://startupweekend.org/">startup weekend</a>-like event where we hack on a product/idea. <a title="We should always work like that (French)" href="http://ogirardot.wordpress.com/2012/09/13/on-devrait-toujours-travailler-comme-ca-hackatonlt/">Last time</a> it was in a nice house in <a title="Gérande, Loire Atlantique, France" href="http://goo.gl/maps/QVqpu">Guérande</a> where we had everything we needed, <strong>internet access</strong>, rooms, tables, lots of space, an indoor swimming pool and a barbecue !
</p>

<p style="text-align:justify;">
  But when you want to find a nice place in France, it's not always easy to also have a good/decent <strong>internet access</strong>, so as we're beginning to plan the next event right now, we asked ourselves what could we do if there was no internet access ? Is there a way to plan for what we would need, so that we wouldn't suffer from having no contact with the outside world :). But in a Java/Python environment, where you use a lot Maven and PyPi, when you don't know what you'll be working on, the one thing you can't (and <strong>shouldn't plan)</strong> is the <strong>libraries/dependencies you'll need.</strong>
</p>

<p style="text-align:justify;">
  So what do we do ? The simplest way is to download all the dependencies you can from a Maven repository but that seems like the most in-efficient way ever, and with more than 30Gb of data each, it can take a while... <strong><br /> </strong>
</p>

<p style="text-align:justify;">
  In the <a title="State of the Maven ecosystem" href="http://ogirardot.wordpress.com/2013/01/11/state-of-the-mavenjava-dependency-graph/">last article</a> I extracted all the libs' metadata and dependencies link, so we know what depends on what. So in order to be more efficient in creating a copied repository, I decided to use those metadata according to two simple rules :
</p>

  * **Only keep the latest version of artifacts;**
  * **And artifacts/versions that are needed to other artifacts in their latest versions.**

With those simple rules, we can create a &#8220;minimum&#8221; repository containing only what we would need to start a new project :). The data I extracted is not perfect so don't take my word on it. This is a first draft of a work I (or someone else) may continue.

The result is a simpler graph containing only **25 553 nodes and 52 916 edges **(compared to the **186 384 Nodes and 1 229 083 Edges **of the full repository), we can almost comprehend :

<figure id="attachment_1003" aria-describedby="caption-attachment-1003" style="width: 640px" class="wp-caption aligncenter">[<img loading="lazy" decoding="async" class="size-large wp-image-1003 " src="http://ogirardot.wordpress.com/wp-content/uploads/2013/01/full-graph-limited-deps-mvn-light.png?w=640" alt="Light version of full-compact maven dependencies - Click to get pdf" width="640" height="640" />][1]<figcaption id="caption-attachment-1003" class="wp-caption-text">Light version of full-compact maven dependencies - Click to get pdf</figcaption></figure>

The full pdf file, almost as good as the svg version (without the 24Mb overhead) is available for download jut by clicking on the picture. But if you need the data because, just like us, you may have to go off the grid, the raw csv file is available on [GitHub here][2]. It's a simple CSV file compressed with LZMA, its columns are _groupId, artifactId, version,_ dependencies, **dependencies** being a base64 encoded json dict.

Hoping you'll enjoy this.

_Vale_

 [1]: http://ogirardot.wordpress.com/wp-content/uploads/2013/01/full-graph-limited-mvn-deps.pdf
 [2]: https://github.com/ogirardot/meta-deps/raw/master/mvn-minimal-deps.csv.lzma "Maven minimal dependencies"