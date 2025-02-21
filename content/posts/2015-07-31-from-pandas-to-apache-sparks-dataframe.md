---
title: From Pandas to Apache Spark’s Dataframe
author: ogirardot
type: post
date: 2015-07-31T21:04:36+00:00

publicize_linkedin_url:
  - 'https://www.linkedin.com/updates?discuss=&scope=33725914&stype=M&topic=6032989062833455104&type=U&a=eG9I'
draftfeedback_requests:
  - 'a:1:{s:13:"55904260a2a8c";a:3:{s:3:"key";s:13:"55904260a2a8c";s:4:"time";s:10:"1435517536";s:7:"user_id";s:8:"28032234";}}'
publicize_twitter_user:
  - ogirardot
categories:
  - Apache Spark
  - BigData
  - Data
  - OSS
  - Python
tags:
  - code
  - Window

---
<!--more-->
<p style="text-align:justify;">
  With the introduction in Spark 1.4 of Window operations, you can finally port pretty much any relevant piece of Pandas' Dataframe computation to Apache Spark parallel computation framework using Spark SQL's Dataframe. If you're not yet familiar with Spark's Dataframe, don't hesitate to checkout my last article <a title="RDDs are the new bytecode of Apache Spark" href="https://ogirardot.wordpress.com/2015/05/29/rdds-are-the-new-bytecode-of-apache-spark/">RDDs are the new bytecode of Apache Spark</a> and come back here after :p.
</p>

<p style="text-align:justify;">
  I figured some feedback on how to port existing &#8220;complex&#8221; code might be useful so the goal of this article will be to take a few concepts from Pandas Dataframe and see how we can translate this to PySpark's Dataframe using Spark > 1.4.
</p>

<p style="text-align:justify;">
  <span style="text-decoration:underline;"><strong>Disclaimer</strong></span>:  a few operations that you can do in Pandas don't have any sense using Spark. Please remember that Dataframes in Spark are like RDD in the sense that they're an immutable data structure. Therefore things like :
</p>

```python
df['three'] = df['one'] * df['two'] # to create a new column "three"  
```

Can't exist, just because this kind of affectation goes against the principles of Spark. Another example would be trying to access by index a single element within a Dataframe. Don't forget that you're using a distributed data structure, not an in-memory random-access data structure.

To be clear, this doesn't mean that you can't do the same kind of thing (i.e. create a new column) using Spark, it means that you have to think immutable/distributed and re-write parts of your code, mostly the parts that are not purely thought-of as transformations on a stream of data.

So let's dive in.

<h2 style="text-align:justify;">
  Column selection
</h2>

This part is not that much different in Pandas and Spark, but you have to take into account the immutable character of your dataframe. First let's create two dataframes one in Pandas \*pdf\* and one in Spark \*df\* :

```python
# Pandas => pdf  
pdf = pd.DataFrame.from_items([('A', [1, 2, 3]), ('B', [4, 5, 6])])

In [18]: pdf.A  
Out[18]:  
0 1  
1 2  
2 3  
Name: A, dtype: int64

\# SPARK SQL => df  
In [19]: df = sqlCtx.createDataFrame([(1, 4), (2, 5), (3, 6)], ["A", "B"])

In [20]: df  
Out[20]: DataFrame[A: bigint, B: bigint]

In [21]: df.show()  
+-+-+  
|A|B|  
+-+-+  
|1|4|  
|2|5|  
|3|6|  
+-+-+  
```

Now in Spark SQL or Pandas you use the same syntax to refer to a column :

```python
In [27]: df.A  
Out[27]: Column<A>

In [28]: df['A']  
Out[28]: Column<A>

In [29]: pdf.A  
Out[29]:  
0 1  
1 2  
2 3  
Name: A, dtype: int64

In [30]: pdf['A']  
Out[30]:  
0 1  
1 2  
2 3  
Name: A, dtype: int64  
```

The output seems different, but these are still the same ways of referencing a column using Pandas or Spark, the only difference is that in Pandas, it is a mutable data structure that you can change, not in Spark.

<h2 style="text-align:justify;">
  Column adding
</h2>

```python
In [31]: pdf['C'] = 0

In [32]: pdf  
Out[32]:  
A B C  
0 1 4 0  
1 2 5 0  
2 3 6 0

\# In Spark SQL you'll use the withColumn or the select method,  
\# but you need to create a "Column", a simple int won't do :  
In [33]: df.withColumn('C', 0)  
-------------------------  
AttributeError Traceback (most recent call last)  
<ipython-input-33-fd1261f623cf> in <module>()  
--> 1 df.withColumn('C', 0)

/Users/ogirardot/Downloads/spark-1.4.0-bin-hadoop2.4/python/pyspark/sql/dataframe.pyc in withColumn(self, colName, col)  
1196 """  
-> 1197 return self.select('*', col.alias(colName))  
1198  
1199 @ignore\_unicode\_prefix

AttributeError: 'int' object has no attribute 'alias'

\# Here's your new best friend "pyspark.sql.functions.*"  
\# If you can't create it from composing columns  
\# this package contains all the functions you'll need :  
In [35]: from pyspark.sql import functions as F  
In [36]: df.withColumn('C', F.lit(0))  
Out[36]: DataFrame[A: bigint, B: bigint, C: int]

In [37]: df.withColumn('C', F.lit(0)).show()  
+-+-+-+  
|A|B|C|  
+-+-+-+  
|1|4|0|  
|2|5|0|  
|3|6|0|  
+-+-+-+  
```

Most of the time in Spark SQL you can use Strings to reference columns but there are two cases where you'll want to use the Column objects rather than Strings :

  * In Spark SQL Dataframe columns are allowed to have the same name, they'll be given unique names inside of Spark SQL, but this means that you can't reference them with the column name only as this becomes ambiguous.
  * When you need to manipulate columns using expressions like **&#8220;Adding two columns to each other&#8221;**, **&#8220;Twice the value of this column&#8221;** or even **&#8220;Is the column value larger than 0 ?&#8221;**, you won't be able to use simple strings and will need the Column reference
  * Finally if you need renaming, cast or any other complex feature, you'll need the Column reference too.

Here's an example :

```python
In [39]: df.withColumn('C', df.A * 2)  
Out[39]: DataFrame[A: bigint, B: bigint, C: bigint]

In [40]: df.withColumn('C', df.A * 2).show()  
+-+-+-+  
|A|B|C|  
+-+-+-+  
|1|4|2|  
|2|5|4|  
|3|6|6|  
+-+-+-+

In [41]: df.withColumn('C', df.B > 0).show()  
+-+-+--+  
|A|B| C|  
+-+-+--+  
|1|4|true|  
|2|5|true|  
|3|6|true|  
+-+-+--+

```

When you're selecting columns, to create another \*projected\* dataframe, you can also use expressions :

```python
In [42]: df.select(df.B > 0)  
Out[42]: DataFrame[(B > 0): boolean]

In [43]: df.select(df.B > 0).show()  
+---+  
|(B > 0)|  
+---+  
| true|  
| true|  
| true|  
+---+  
```

As you can see the column name will actually be computed according to the expression you defined, if you want to rename this, you'll need to use the **alias **method on Column :

```python
In [44]: df.select((df.B > 0).alias("is_positive")).show()  
+----+  
|is_positive|  
+----+  
| true|  
| true|  
| true|  
+----+  
```

All of the expressions that we're building here can be used for Filtering, Adding a new column or even inside Aggregations, so once you get a general idea of how it works, you'll be fluent throughout all of the Dataframe manipulation framework.

## Filtering

Filtering is pretty much straightforward too, you can use the \*RDD-like\* **filter **method and copy any of your existing Pandas expression/predicate for filtering :

```python
In [48]: pdf[(pdf.B > 0) & (pdf.A < 2)]  
Out[48]:  
A B C  
0 1 4 0

In [49]: df.filter((df.B > 0) & (df.A < 2)).show()  
+-+-+  
|A|B|  
+-+-+  
|1|4|  
+-+-+

In [55]: df[(df.B > 0) & (df.A < 2)].show()  
+-+-+  
|A|B|  
+-+-+  
|1|4|  
+-+-+  
```

## Aggregations

What can be confusing at first in using aggregations is that the minute you write **groupBy **you're not using a Dataframe object, you're actually using a **GroupedData **object and you need to precise your aggregations to get back the output Dataframe :

```python
In [77]: df.groupBy("A")  
Out[77]: <pyspark.sql.group.GroupedData at 0x10dd11d90>

In [78]: df.groupBy("A").avg("B")  
Out[78]: DataFrame[A: bigint, AVG(B): double]

In [79]: df.groupBy("A").avg("B").show()  
+-+--+  
|A|AVG(B)|  
+-+--+  
|1| 4.0|  
|2| 5.0|  
|3| 6.0|  
+-+--+  
```

As a syntactic sugar if you need only one aggregation, you can use the simplest functions like : **avg, cout, max, min, mean** and **sum **directly on GroupedData, but most of the time, this will be too simple and you'll want to create a few aggregations during a single groupBy operation. After all (c.f. [RDDs are the new bytecode of Apache Spark][1] ) this is one of the greatest features of the Dataframes. To do so you'll be using the **agg **method :

```python
In [83]: df.groupBy("A").agg(F.avg("B"), F.min("B"), F.max("B")).show()  
+-+--+--+--+  
|A|AVG(B)|MIN(B)|MAX(B)|  
+-+--+--+--+  
|1| 4.0| 4| 4|  
|2| 5.0| 5| 5|  
|3| 6.0| 6| 6|  
+-+--+--+--+  
```

Of course, just like before, you can use any expression especially column compositions, alias definitions etc... and some other non-trivial functions :

```python
In [84]: df.groupBy("A").agg(  
....: F.first("B").alias("my first"),  
....: F.last("B").alias("my last"),  
....: F.sum("B").alias("my everything")  
....: ).show()  
+-+---+---+-----+  
|A|my first|my last|my everything|  
+-+---+---+-----+  
|1| 4| 4| 4|  
|2| 5| 5| 5|  
|3| 6| 6| 6|  
+-+---+---+-----+  
```

## Complex operations & Windows

Now that Spark 1.4 is out, the Dataframe API provides an efficient and easy to use Window-based framework - this single feature is what makes any Pandas to Spark migration actually do-able for 99% of the projects - even considering some of Pandas' features that seemed \*hard\* to reproduce in a distributed environment.

A simple example that we can pick is that in Pandas you can compute a **diff ** on a column and Pandas will compare the values of one line to the last one and compute the difference between them. Typically the kind of feature hard to do in a distributed environment because each line is supposed to be treated independently, now with Spark 1.4 window operations you can define a window on which Spark will &#8220;**execute some aggregation functions&#8221;** but relatively to a specific line. Here's how to port some existing Pandas code using diff :

```python
In [86]: df = sqlCtx.createDataFrame([(1, 4), (1, 5), (2, 6), (2, 6), (3, 0)], ["A", "B"])

In [95]: pdf = df.toPandas()

In [96]: pdf  
Out[96]:  
A B  
0 1 4  
1 1 5  
2 2 6  
3 2 6  
4 3 0

In [98]: pdf['diff'] = pdf.B.diff()

In [102]: pdf  
Out[102]:  
A B diff  
0 1 4 NaN  
1 1 5 1  
2 2 6 1  
3 2 6 0  
4 3 0 -6  
```

In Pandas you can compute a diff on an arbitrary column, with no regard for keys, no regards for order or anything. It's cool... but most of the time not exactly what you want and you might end up cleaning up the mess afterwards by setting the column value back to NaN from one line to another when the keys changed.

Here's how you can do such a thing in PySpark using Window functions, a Key and, if you want, in a specific Order :

```python
In [107]: from pyspark.sql.window import Window

In [108]: window\_over\_A = Window.partitionBy("A").orderBy("B")

In [109]: df.withColumn("diff", F.lead("B").over(window\_over\_A) - df.B).show()  
+-+-+ 
| A| B|diff|  
+-+-+ 
| 1| 4| 1|  
| 1| 5|null|  
| 2| 6| 0|  
| 2| 6|null|  
| 3| 0|null|  
+-+-+ 
```

With that you are now able to compute a diff line by line - ordered or not - given a specific key. The great point about Window operation is that you're not actually breaking the structure of your data. Let me explain myself.

When you're computing some kind of aggregation (once again according to a key), you'll usually be executing a **groupBy** operation given this key and compute the multiple metrics that you'll need (if you're lucky \*at the same time\*, if you're not in multiple **reduceByKey** or **aggregateByKey** transformations).

But whether you're using RDDs or Dataframe, if you're not using window operations then you'll actually crush your data in a part of your flow and then you'll need to join back again the results of your aggregations to the \*main\*-dataflow. Window operations allows you to execute your computation and copy the results as additional columns without any explicit join.

This is a quick way to enrich your data adding rolling computations as just another column directly. Two additional resources are worth noting regarding these new features, the official [Databricks blog article on Window operations][2] and [Christophe Bourguignat][3]'s article evaluating [Pandas and Spark Dataframe differences][4].

To sum up you now have all the tools you need in Spark > 1.4 to port any Pandas computation in a distributed environment using the \*very\* similar Dataframe API.

_Vale_

 [1]: https://ogirardot.wordpress.com/2015/05/29/rdds-are-the-new-bytecode-of-apache-spark/ "RDDs are the new bytecode of Apache Spark"
 [2]: https://databricks.com/blog/2015/07/15/introducing-window-functions-in-spark-sql.html
 [3]: http://twitter.com/chris_bour
 [4]: https://medium.com/@chris_bour/6-differences-between-pandas-and-spark-dataframes-1380cec394d2