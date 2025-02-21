---
title: Elasticsearch is the way
author: ogirardot
type: post
date: 2013-03-12T20:34:05+00:00

publicize_twitter_user:
  - ogirardot
publicize_reach:
  - 'a:2:{s:7:"twitter";a:1:{i:2397699;i:363;}s:2:"wp";a:1:{i:0;i:0;}}'
categories:
  - BigData
  - Data
  - Elasticsearch
tags:
  - elasticsearch
  - search engine
  - solr

---
<p style="text-align:justify;">
  Don't get me wrong, i love <a title="Solr" href="http://lucene.apache.org/solr/">Apache Solr</a>, i think it's a wonderful project and the versions 4.x are definitely something you should check out when building a proper search engine.
</p>
<!--more-->

<p style="text-align:justify;">
  <strong>But</strong> Elasticsearch, at least for me, is now the way to the future. If you need a few reasons why, read on :
</p>

<h2 style="text-align:justify;">
  Out of the box scalability
</h2>

<p style="text-align:justify;">
  SolrCloud is doing a good job trying to get Solr into the Cloud era, because even if Solr supported distributed query before, sharding had to be done manually...
</p>

<p style="text-align:justify;">
  Elasticsearch scalability is so easy it's a bit frightening, every time i set up a new elasticsearch &#8220;single&#8221; server i deactivate as soon as possible the cluster-search capability, just in case it starts replicating the internet on my machine !  Sharding/Replication is automatic and almost a necessity, because your server (by default) will remind you that you're a dangerous person keeping all your data on a single machine, and will stay in a <strong>yellow state</strong> until you start adding some nodes !
</p>

<h2 style="text-align:justify;">
  Comprehensive Json-based HTTP search API
</h2>

<p style="text-align:justify;">
  In all honesty sometimes the json-based search queries can become quite complicated and tedious to read, but it's much more powerful than a simple <strong>?q=.... </strong> query or the long and complicated list of URL-GET parameters you end up using with Solr... So even if there are no proper Chrome extension to create a GET HTTP request with a JSON body (!! add a comment if you find one !!), i still think it's a blessing to have that kind of query capacity, and it made me rethink about elasticsearch's tolerance/suitability for complex query (c.f.  the &#8220;As complex as Solr&#8221; part).
</p>

<h2 style="text-align:justify;">
  Rivers...
</h2>

<p style="text-align:justify;">
  Probably one of the best feature of Elasticsearch, it's designed around the fantastic (and true) idea that an Elasticsearch index needs to be fed !
</p>

<p style="text-align:justify;">
  <img loading="lazy" decoding="async" class="aligncenter" alt="" src="http://funnyasduck.net/wp-content/uploads/2013/02/funny-crazy-cat-feed-me-kill-whole-family-pics.jpg" width="455" height="413" />
</p>

<p style="text-align:justify;">
  Just this concept changes everything, because it makes the <strong>&#8220;realtime index&#8221; </strong>the default type of index, because anyway nowadays what matters most is to have an up-to-date search index and it's a fact that Near-Realtime search is one the many advantages that makes Solr and Elasticsearch the best choices out there.
</p>

<h2 style="text-align:justify;">
  Vibrant community and plugins
</h2>

<p style="text-align:justify;">
  Probably the most important part, in my opinion, i do think that the Solr ecosystem lacks a lot of good tools and plugins to leverage more of its power. <a title="Luke" href="https://code.google.com/p/luke/">Luke</a> is a pretty useful tool, but it's very lucene-centric, apart from the solr-provided tools (which are, i must say, sufficient for a lot troubleshooting and debugging). I've been on Solr 3.x for a long time, and even if all the tools where there, the UI certainly lacked in terms of &#8220;sexy&#8221;, nowadays Solr 4.x's UI is certainly more sexy and a pleasure to work with, but it's still only the work of Lucidworks.
</p>

<p style="text-align:justify;">
  Elasticsearch is brand new, the documentation is sexy, the project is sexy, they built a wonderful plugin system that <strong>uses github directly !! <span style="text-decoration:underline;">You don't have to be a fully accredited &#8220;Elasticsearch-compliant plugin creator&#8221; to publish your project</span></strong>.
</p>

<p style="text-align:justify;">
  So a lot of people created wonderful plugins, that already goes beyond what you can use in the Solr/Lucene world, just a quick review :
</p>

<ul style="text-align:justify;">
  <li>
    <span style="line-height:13px;"><a title="Paramedic" href="https://github.com/karmi/elasticsearch-paramedic">Paramedic</a> : a &#8220;simple and sexy tool to monitor and inspect elasticsearch clusters&#8221;;</span>
  </li>
  <li>
    <a title="Head" href="https://github.com/mobz/elasticsearch-head">Head</a> : &#8220;A web front end for an ElasticSearch cluster&#8221; with a real-time dashboard;
  </li>
  <li>
    <a title="BigDesk" href="https://github.com/lukas-vlcek/bigdesk">BigDesk</a> : Live charts and statistics for Elasticsearch cluster;
  </li>
  <li>
    <p style="display:inline!important;">
      For the analysis, you have <a title="Inquisitor" href="https://github.com/polyfractal/elasticsearch-inquisitor">Inquisitor</a>
    </p>
    
    <p>
      to help understand and debug your queries in ElasticSearch and <a title="SegmentSpy" href="https://github.com/polyfractal/elasticsearch-segmentspy">SegmentSpy</a> to watch real time segments merging and changing.</li> </ul> 
      
      <p style="text-align:justify;">
        This is just the state of the art right now, but i can't imagine it going anywhere but forward.
      </p>
      
      <h2 style="text-align:justify;">
        As complex as Solr
      </h2>
      
      <p style="text-align:justify;">
        Finally, i had prejudiced, because i thought that the goals of Elasticsearch in terms of scalability where clearly ambitious (and deeply needed !), but that <span style="text-decoration:underline;">this kind of scalability obviously came at a cost and therefor there would be less features than what Solr offered</span> (ex. <a title="Dismax" href="http://wiki.apache.org/solr/DisMax">Dismax</a> queries).
      </p>
      
      <p style="text-align:justify;">
        But <span style="text-decoration:underline;"><strong>i was</strong><strong> wrong</strong></span>, as i discovered recently that Dismax queries, fuzzy matching and other goodies allowing many things from <em>boosted-field at query time</em> to <em>boosted sub-queries</em>, are available and easily accessible thanks to the Elasticsearch API. So the proper section-name should not be &#8220;As complex as Solr&#8221; but <strong>&#8220;As versatile as Solr&#8221;.</strong>
      </p>
      
      <p style="text-align:justify;">
        I hope i made my point, and if you're considering building a BigData-ready search engine right now, make sure to check out <a href="http://elasticsearch.org">Elasticsearch</a> or you'll be missing out on a great product.
      </p>
      
      <p style="text-align:justify;">
        <em>Vale</em>
      </p>