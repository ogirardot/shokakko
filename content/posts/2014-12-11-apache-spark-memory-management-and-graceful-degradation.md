---
title: 'Apache Spark : Memory management and Graceful degradation'
author: ogirardot
type: post
date: 2014-12-11T13:30:00+00:00

publicize_twitter_user:
  - ogirardot
publicize_twitter_url:
  - http://t.co/INjgCGonBQ
publicize_linkedin_url:
  - 'https://www.linkedin.com/updates?discuss=&scope=33725914&stype=M&topic=5948800852889210880&type=U&a=IMbg'
categories:
  - Apache Spark
  - BigData
tags:
  - Memory
  - RAM

---
<p style="text-align:justify;">
  Many of the concepts of Apache Spark are pretty straightforward and easy to understand, however some lucky few can be badly misunderstood. One of the greatest misunderstanding of all is the fact that some still believe that &#8220;<em>Spark is only relevant with datasets that can fit into memory, otherwise it will crash&#8221;</em>.
</p>

<p style="text-align:justify;">
  This is an understanding mistake, Spark being easily associated as a &#8220;Hadoop using RAM more efficiently&#8221;, but it still is a mistake.
</p>

<p style="text-align:justify;">
  Spark is by default doing the best it can to load the datasets it handles in memory. Still when the handled datasets are too large to fit into memory, automatically (or should i say auto-magically) these objects will be spilled to disk. This is one of the main features of Spark coined by the expression &#8220;<strong>graceful </strong><b>degradation&#8221;</b> and it was very well illustrated by these two charts in <a title="An Architecture for Fast and General Data Processing on Large Clusters" href="http://www.eecs.berkeley.edu/Pubs/TechRpts/2014/EECS-2014-12.pdf" target="_blank">Matei Zaharia's dissertation : An Architecture for Fast and General Data Processing on Large Clusters</a> :
</p>

<figure id="attachment_1211" aria-describedby="caption-attachment-1211" style="width: 660px" class="wp-caption aligncenter">[<img loading="lazy" decoding="async" class="wp-image-1211 size-large" src="https://ogirardot.wordpress.com/wp-content/uploads/2014/11/graceful-degradation-spark.png?w=660" alt="Behaviour of Spark with less/more RAM, extracted from http://www.eecs.berkeley.edu/Pubs/TechRpts/2014/EECS-2014-12.pdf" width="660" height="496" />][1]<figcaption id="caption-attachment-1211" class="wp-caption-text">Behaviour of Spark with less/more RAM, extracted from http://www.eecs.berkeley.edu/Pubs/TechRpts/2014/EECS-2014-12.pdf</figcaption></figure>

So the first chart clearly shows something interesting for us, It shows us that the behavior of Spark when you give it more or less RAM is pretty much linear in terms of execution time. In other words, the more RAM Spark can use, the quicker your computation will run, but if you give it less and less RAM, in the end Spark will behave like Hadoop, flushing to disk as much as possible.

The second chart is also interesting for debunking the urban legend of &#8220;Spark will only work if your datasets fit in RAM&#8221; showing how Spark will handle larger and larger datasets, once again its behavior is practically linear between the time the computation takes and the size of the dataset (for a given computation).

In the end, not only Spark can handle large datasets but It will gracefully adapt to the amount of memory you give it.

 [1]: https://ogirardot.wordpress.com/wp-content/uploads/2014/11/graceful-degradation-spark.png