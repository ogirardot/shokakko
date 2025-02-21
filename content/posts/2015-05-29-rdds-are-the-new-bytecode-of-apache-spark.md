---
title: RDDs are the new bytecode of Apache Spark
author: ogirardot
type: post
date: 2015-05-29T20:09:50+00:00

publicize_linkedin_url:
  - 'https://www.linkedin.com/updates?discuss=&scope=33725914&stype=M&topic=6010144844515721216&type=U&a=dQSW'
publicize_twitter_user:
  - ogirardot
categories:
  - Apache Spark
  - BigData
  - Data
tags:
  - Dataframe
  - Pandas
  - RDD

---
<p style="text-align:justify;">
  With the Apache Spark 1.3 release the Dataframe API for Spark SQL got introduced, for those of you who missed the big announcements, I'd recommend to read the article : <a title="Introducing Dataframes in Spark for Large Scale Data Science" href="https://databricks.com/blog/2015/02/17/introducing-dataframes-in-spark-for-large-scale-data-science.html" target="_blank">Introducing Dataframes in Spark for Large Scale Data Science</a> from the Databricks blog. Dataframes are very popular among data scientists, personally I've mainly been using them with the great Python library <a title="Pandas" href="http://pandas.pydata.org" target="_blank">Pandas</a> but there are many examples in R (originally) and Julia.
</p>
<!--more-->

<p style="text-align:justify;">
  Of course if you're using only Spark's core features, nothing seems to have changed with Spark 1.3 : Spark's main abstraction remains the RDD (Resilient Distributed Dataset), its API is very stable now and everyone used it to handle any kind of data since now.
</p>

<p style="text-align:justify;">
  But the introduction of Dataframe is actually a big deal, because when RDDs were the only option to load data, it was obvious that you needed to parse your &#8220;maybe&#8221; un-structured data using RDDs, transform them using case-classes or tuples and then do the special work that you actually needed. Spark SQL is not a new project and you were, of course, able to load your structured-data (like Parquet files) directly from a <a title="SQLContext in Apache Spark 1.0" href="https://spark.apache.org/docs/1.0.0/api/scala/index.html#org.apache.spark.sql.SQLContext" target="_blank">SQLContext</a> before 1.3 - but the advantages were not that clear at the time - except if you wanted to run SQL queries or expose a JDBC-compatible server for other BI tools.
</p>

<p style="text-align:justify;">
  Now the advantages are quite clear and I'll try to explain them as simply as possible :
</p>

<h2 style="text-align:justify;">
  1) Dataframes are a higher level of abstraction than RDDs
</h2>

<p style="text-align:justify;">
  If you're familiar with Pandas syntax, you will feel at home using Spark's Dataframe and even if you're not, you'll learn and - I'd even add - learn to love it. Why ? Because it's a higher level of programming than the RDD, you can <a title="Oops" href="http://www.domorefasterbook.com/" target="_blank">do more, faster</a> (old joke now 😉 ). Here's an example from <a title="Patrick Wendell" href="http://www.pwendell.com/" target="_blank">Patrick Wendell</a>'s Strata London 2015 presentation &#8220;What's coming in Spark&#8221; of RDDs in Python vs Dataframe :
</p>

<p style="text-align:justify;">
  <a href="https://ogirardot.wordpress.com/wp-content/uploads/2015/05/rdd-vs-dataframe.png"><img loading="lazy" decoding="async" class="aligncenter wp-image-1265 size-large" src="https://ogirardot.wordpress.com/wp-content/uploads/2015/05/rdd-vs-dataframe.png?w=660" alt="RDD vs Dataframe" width="660" height="371" /></a>
</p>

<p style="text-align:justify;">
  Of course the second way of writing it is obviously more concise and more understandable, but I'd like to add something else, the <em>tried-and-tested</em> Spark programmers have surely noticed the <strong>reduceByKey </strong>transformation used here. It is a very common mistake in Spark for common aggregation tasks to use the <strong>groupBy </strong>then <strong>mapValues </strong>or <strong>map</strong> transformation which can be dangerous in a production environment and produce <strong>OutOfMemory </strong>errors on workers.
</p>

<p style="text-align:justify;">
  Do you notice that such a mistake <strong>cannot </strong>happen using the Dataframe API below for you will be expressing your aggregations using, for example, the <strong>agg(...)</strong> method (or even directly the <strong>avg(...) </strong>method like up there). This will even allow you to define multiple aggregations at the same time, something that is usually tricky using RDDs :
</p>

[code language=&#8221;scala&#8221;]  
case class Person(id: Int, first\_name: String, last\_name: String, age: Double)

// get simple stats on age repartitions by first_name(min, max, avg, count)  
val rdd: RDD[Person] = ...  
// first you need to only handle the data you really need, and cache it because you'll - sadly - reuse it  
val persons = rdd.map(person => (person.first_name, person.age)).cache()

val minAgeByFirstName = persons.reduceByKey( scala.math.min(\_, \_) )  
val maxAgeByFirstName = persons.reduceByKey( scala.math.max(\_, \_) )  
val avgAgeByFirstName = persons.mapValues(x => (x, 1))  
.reduceByKey((x, y) => (x.\_1 + y.\_1, x.\_2 + y.\_2)) // simple right ?  
val countByFirstName = persons.mapValues(x => 1).reduceByKey(_ + _)  
[/code]

<p style="text-align:justify;">
  Without even trying to consider the complexity of all I had to write to get all my answers - answers that I would need to join back if I want a consistent RDD with all the informations I need - the most painful point is that I had to duplicate all these aggregations and therefore <strong>cache</strong> my dataset to mitigate the damages.
</p>

<p style="text-align:justify;">
  Now using the dataframe API, I get to leverage out-of-the-box functions and I can even reuse my computations afterward without having to join-back anything :
</p>

[code language=&#8221;scala&#8221;]  
case class Person(id: Int, first\_name: String, last\_name: String, age: Double)

// get simple stats on age repartitions by first_name(min, max, avg, count)  
val df: Dataframe = ...  
persons = df.groupBy("first_name").agg(  
min("age").alias("min_age"),  
max("age").alias("max_age"),  
avg("age").alias("average_age"),  
count("*").alias("number\_of\_persons")  
)  
// let's add a new column to our schema re-using the two last-computed aggregations :  
val finalDf = persons.withColumn("age\_delta", persons("max\_age") - persons("min_age"))  
[/code]

<p style="text-align:justify;">
  This is a higher level of programming than RDDs, so some things might be more difficult to express with Dataframe than they were using RDDs when you could <strong>groupBy(...)</strong> anything and get the <em>List[...]</em> of result as values... But this was a bad practice anyway :).
</p>

<h2 style="text-align:justify;">
  2) Spark SQL/Catalyst is more intelligent than you
</h2>

<p style="text-align:justify;">
  When you're using Dataframe, you're not defining directly a DAG (Directed Acyclic Graph) anymore, you're actually creating an AST (Abstract Syntax Tree) that the Catalyst engine will parse, check and improve using both Rules-Based Optimisation and Cost-Based Optimisation. This is an excerpt from the Spark SQL paper submitted for SIGMOD 2015 :
</p>

<p style="text-align:justify;">
  <a href="https://ogirardot.wordpress.com/wp-content/uploads/2015/05/spark-sql-pipeline.png"><img loading="lazy" decoding="async" class="aligncenter size-large wp-image-1269" src="https://ogirardot.wordpress.com/wp-content/uploads/2015/05/spark-sql-pipeline.png?w=660" alt="spark SQL pipeline" width="660" height="200" /></a>
</p>

<p style="text-align:justify;">
  I won't get into the depth of this here, because that would even need more than one full article about it, but if you want to understand more this article <a href="https://databricks.com/blog/2015/04/13/deep-dive-into-spark-sqls-catalyst-optimizer.html" target="_blank">Deep dive into Spark SQL's Catalyst optimizer</a> from the Databricks blog (once again) will give you insights into how this works. A simple rule if thumb to get is that a lot of &#8220;pretty logical&#8221; generic tree-based rules will be used to check and simplify your parsed-Logical Plan and then a few Physical Plans representing different executions strategies will be computed and one will be selected according to their &#8220;computation cost&#8221;.
</p>

<p style="text-align:justify;">
  The funny thing is that in the end - nothing changes - after all these transformations your Dataframe will get *compiled* down to RDDs and executed on your Spark Cluster.
</p>

<h2 style="text-align:justify;">
  3) Python & Scala are now even in terms of performance
</h2>

<p style="text-align:justify;">
  Using the Dataframe API, you're using a DSL that leverages Spark's Scala bytecode - when using RDDs, Python lambdas will run in a Python VM, Java/Scala lambdas will run in the JVM, this is great because inside RDDs you can use your usual Python libraries (Numpy, Scipy, etc...) and not some Jython code, but it comes at a performance cost :
</p>

<p style="text-align:justify;">
  <a href="https://ogirardot.wordpress.com/wp-content/uploads/2015/05/unified-physical-execution.png"><img loading="lazy" decoding="async" class="aligncenter size-large wp-image-1268" src="https://ogirardot.wordpress.com/wp-content/uploads/2015/05/unified-physical-execution.png?w=660" alt="Unified physical execution" width="660" height="370" /></a>
</p>

<p style="text-align:justify;">
  This is still true if you want to use Dataframe's User Defined Functions, you can write them in Java/Scala or Python and this will impact your computation performance - but if you manage to stay in a pure Dataframe computation - then nothing will get between you and the best computation performance you can possibly get.
</p>

<h2 style="text-align:justify;">
  4)  Dataframes are the future for Spark & You
</h2>

<p style="text-align:justify;">
  Spark ML is already a pretty obvious example of this, the Pipeline API is designed entirely around Dataframes as their sole data structure for parallel computations, model training and predictions. And even if you don't believe me, here's once again Patrick Wendell's presentation of &#8220;What the future of Spark is&#8221; :
</p>

<p style="text-align:justify;">
  <a href="https://ogirardot.wordpress.com/wp-content/uploads/2015/05/future-of-spark.png"><img loading="lazy" decoding="async" class="aligncenter size-large wp-image-1270" src="https://ogirardot.wordpress.com/wp-content/uploads/2015/05/future-of-spark.png?w=660" alt="Future of Spark" width="660" height="369" /></a>
</p>

<p style="text-align:justify;">
  Anyway, I think I made my point regarding the whole goal of this article : RDDs are the new bytecode of Apache Spark. You might be sad or pissed because you spent a lot of time learning how to harness Spark's RDDs and now you think Dataframes are a completely new paradigm to learn...
</p>

<p style="text-align:justify;">
  You're partially right because if you don't already know Pandas or R API, Dataframes are a new thing and you'll need some work to harness it - but remember that in the end, everything comes down as RDDs - so all that you learned before is still relevant, this is just another skill to add to your resume.
</p>

<p style="text-align:justify;">
  <em>Vale</em>
</p>