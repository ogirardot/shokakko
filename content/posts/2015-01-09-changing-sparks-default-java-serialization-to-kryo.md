---
title: Changing Spark’s default java serialization to Kryo
author: ogirardot
type: post
date: 2015-01-09T14:00:00+00:00

publicize_twitter_url:
  - http://t.co/8P6rfcaD7q
publicize_twitter_user:
  - ogirardot
categories:
  - Apache Spark
  - BigData
  - Data
  - Java
tags:
  - Kryo
  - Serialization

---
<p style="text-align:justify;">
  Apache Spark's default serialization relies on Java with the default <em>readObject(...) </em>and <em>writeObject(...) </em> methods for all <strong>Serializable </strong>classes. This is a very fine default behavior as long as you don't rely on it too much...
</p>
<!--more-->

<p style="text-align:justify;">
  Why ? Because Java's serialization framework is notoriously inefficient, consuming too much CPU, RAM and size to be a suitable large scale serialization format.
</p>

<p style="text-align:justify;">
  Ok, but you can always tell me that you, as a Apache Spark user, are not using Java's serialization framework at all, but the fact is that Apache Spark as a system relies on it a lot :
</p>

<ul style="text-align:justify;">
  <li>
    Every task run from Driver to Worker gets serialized : <strong>Closure serialization</strong>
  </li>
  <li>
    Every result from every task gets serialized at some point : <strong>Result serialization</strong>
  </li>
</ul>

<p style="text-align:justify;">
  And what's implied is that during all <strong>c</strong><strong>losure serializations</strong> all the <strong>values used inside</strong> will get serialized as well, for the record, this is also one of the main reasons to use Broadcast variables when closures might get serialized with big values.
</p>

<p style="text-align:justify;">
  <a title="Kryo - Java serialization" href="https://github.com/EsotericSoftware/kryo">Kryo</a> is a project like <a href="http://avro.apache.org/">Apache Avro</a> or <a href="https://github.com/google/protobuf/">Google's Protobuf</a> (or it's Java oriented equivalent <a href="https://github.com/protostuff/protostuff">Protostuff</a> - which I have not tested yet). I'm not a bug fan of benchmarks but they can be useful and Kryo designed a few to measure size and time of serialization. Here's what such a benchmark looks like a the time of writing (i.e early 2015) :
</p>

<p style="text-align:justify;">
  <img loading="lazy" decoding="async" class=" aligncenter" src="https://camo.githubusercontent.com/829809b59ac2efe1ec62ac2f2cfbb29606a02a44/68747470733a2f2f63686172742e676f6f676c65617069732e636f6d2f63686172743f636874743d746f74616c2b2532386e616e6f73253239266368663d637c7c6c677c7c307c7c4646464646467c7c317c7c3736413446427c7c307c62677c7c737c7c454645464546266368733d35303078343330266368643d743a313232362c313439322c313536382c323230302c323436352c323933392c333530312c333635392c333637302c343439352c383531362c31303035372c31303437372c31323138372c31333130392c31353632382c31393938302c32383534382c33363034362c343438333826636864733d302c34393332322e313530333526636878743d79266368786c3d303a7c6a736f6e253246666c65786a736f6e2532466461746162696e647c6a6176612d6275696c742d696e7c6a626f73732d6d61727368616c6c696e672d72697665727c786d6c2532467873747265616d253242637c6a736f6e2532467376656e736f6e2d6461746162696e647c6a626f73732d73657269616c697a6174696f6e7c62736f6e2532466a61636b736f6e2532466461746162696e647c6a736f6e253246676f6f676c652d67736f6e2532466461746162696e647c6865737369616e7c786d6c2532466a61636b736f6e2532466461746162696e642d61616c746f7c6a736f6e2532466a61636b736f6e2532466461746162696e647c6a736f6e25324670726f746f73747566662d72756e74696d657c736d696c652532466a61636b736f6e2532466461746162696e647c6a736f6e2532466a61636b736f6e25324664622d61667465726275726e65727c736d696c652532466a61636b736f6e25324664622d61667465726275726e65727c6a736f6e253246666173746a736f6e2532466461746162696e647c6d73677061636b2d6461746162696e647c666173742d73657269616c697a6174696f6e7c6b72796f7c70726f746f73747566662663686d3d4e2532302a662a2c3030303030302c302c2d312c3130266c6b6c6b266368646c703d74266368636f3d3636303030307c3636303033337c3636303036367c3636303039397c3636303043437c3636303046467c3636333330307c3636333333337c3636333336367c3636333339397c3636333343437c3636333346467c3636363630307c3636363633337c363636363636266368743d62686726636862683d31302c302c3130266e6f6e73656e73653d6161612e706e67" alt="" width="500" height="430" />
</p>

<p style="text-align:justify;">
  So how can you change Spark's default serializer easily, well, as usual Spark is a pretty configurable system, so all you need is to specify which serializer you want to use when you define your <a href="https://spark.apache.org/docs/latest/api/scala/index.html#org.apache.spark.SparkContext">SparkContext</a> using the <a href="https://spark.apache.org/docs/latest/api/scala/index.html#org.apache.spark.SparkConf">SparkConf</a> like that :
</p>

[code language=&#8221;scala&#8221;]  
val conf = new SparkConf()  
.set("spark.serializer", "org.apache.spark.serializer.KryoSerializer")  
[/code]

<p style="text-align:justify;">
  And voilà ! But that's not all, if you've got big objects to serializer and are prepared to <strong>face the consequences</strong> you might get OutOfMemoryErrors or GC Overflows that will happen very fast using Java's default serialization (did I tell you it sucks for some reasons... ?) and won't get resolve auto-magically by switching to Kryo.
</p>

<p style="text-align:justify;">
  Luckily you can define what buffer size Kryo will use by default :
</p>

[code language=&#8221;scala&#8221;]  
val conf = new SparkConf()  
.set("spark.serializer", "org.apache.spark.serializer.KryoSerializer")  
// Now it's 24 Mb of buffer by default instead of 0.064 Mb  
.set("spark.kryoserializer.buffer.mb","24")  
[/code]

<p style="text-align:justify;">
  If you're even bolder you can customize all of these options :
</p>

<ul style="text-align:justify;">
  <li>
    <strong>spark.kryoserializer.buffer.max.mb </strong>(64 Mb by default) : useful if your default buffer size goes further than 64 Mb;
  </li>
  <li>
    <strong>spark.kryo.referenceTracking</strong> (true by default) : c.f. <a href="https://github.com/EsotericSoftware/kryo#references">reference tracking in Kryo</a>
  </li>
  <li>
    <strong>spark.kryo.registrationRequired</strong> (false by default) : Kryo's parameter to define if all serializable classes must be registered
  </li>
  <li>
    <strong>spark.kryo.classesToRegister</strong> (empty string list by default) : you can add a list of the qualified names of all classes that must be registered (c.f. last parameter)
  </li>
</ul>

<p style="text-align:justify;">
  The examples above are defined in Scala, but of course these parameters can be used in Java and Python as well.
</p>

<p style="text-align:justify;">
  Enjoy.
</p>