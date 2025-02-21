---
title: Try Apache Spark’s shell using Docker
author: ogirardot
type: post
date: 2014-12-18T14:41:47+00:00

publicize_twitter_user:
  - ogirardot
publicize_twitter_url:
  - http://t.co/IK8CVS9u5E
publicize_linkedin_url:
  - 'https://www.linkedin.com/updates?discuss=&scope=33725914&stype=M&topic=5951355451256897536&type=U&a=P4nc'
categories:
  - Apache Spark
  - BigData
  - Scala
tags:
  - Docker

---
<p style="text-align:justify;">
  Ever wanted to try out <a title="Apache Spark" href="https://spark.apache.org/">Apache Spark</a> without actually having to install anything ? Well if you've got <a title="Docker" href="https://www.docker.com/">Docker</a>, I've got a christmas present for you, a Docker image you can pull to try and run Spark commands in the Spark shell REPL. The image has been pushed to the <a href="https://registry.hub.docker.com/u/ogirardot/spark-docker-shell">Docker Hub here</a> and can be easily pulled using Docker.
</p>

<p style="text-align:justify;">
  So exactly what is this image, and how can I use it ?
</p>

<p style="text-align:justify;">
  Well, all you need is to execute these few commands :
</p>

[code language=&#8221;bash&#8221;]  
> docker pull ogirardot/spark-docker-shell

[/code]

<p style="text-align:justify;">
  I'll try to keep this image up-to-date with future releases of Spark, so if you want to test against a specific version, all you have to do is pull (or directly run) the <a href="https://registry.hub.docker.com/u/ogirardot/spark-docker-shell/tags/manage/">image with the corresponding tag</a> like that :
</p>

[code language=&#8221;bash&#8221;]  
> docker pull ogirardot/spark-docker-shell:1.1.1

[/code]

<p style="text-align:justify;">
  And then after Docker will have downloaded the full image, using the run command you will have access to a stand-alone <strong>spark-shell </strong>that will allow you to try and learn Spark's API in a sandboxed environment, here's what a correct launch looks like :
</p>

[code language=&#8221;scala&#8221;]  
> docker run -t -i ogirardot/spark-docker-shell  
Spark assembly has been built with Hive, including Datanucleus jars on classpath  
Using Spark's default log4j profile: org/apache/spark/log4j-defaults.properties  
14/12/11 20:33:14 INFO SecurityManager: Changing view acls to: root  
14/12/11 20:33:14 INFO SecurityManager: Changing modify acls to: root  
14/12/11 20:33:14 INFO SecurityManager: SecurityManager: authentication disabled; ui acls disabled; users with view permissions: Set(root); users with modify permissions: Set(root)  
14/12/11 20:33:14 INFO HttpServer: Starting HTTP Server  
14/12/11 20:33:14 INFO Utils: Successfully started service 'HTTP class server' on port 50535.  
Welcome to  
\___\_ __  
/ \_\_/\_\_ \___ __\_\\_\_/ /\_\_  
\_\ \/ \_ \/ _ \`/ _\_/ '\_/  
/\_\\_\_/ .\_\_/\_,\_/\_/ /\_/\\_\ version 1.1.1  
/_/

Using Scala version 2.10.4 (OpenJDK 64-Bit Server VM, Java 1.7.0_65)  
Type in expressions to have them evaluated.  
Type :help for more information.  
14/12/11 20:33:18 INFO SecurityManager: Changing view acls to: root  
14/12/11 20:33:18 INFO SecurityManager: Changing modify acls to: root  
14/12/11 20:33:18 INFO SecurityManager: SecurityManager: authentication disabled; ui acls disabled; users with view permissions: Set(root); users with modify permissions: Set(root)  
14/12/11 20:33:19 INFO Slf4jLogger: Slf4jLogger started  
14/12/11 20:33:19 INFO Remoting: Starting remoting  
14/12/11 20:33:19 INFO Remoting: Remoting started; listening on addresses :[akka.tcp://sparkDriver@ea9ec670e429:43346]  
14/12/11 20:33:19 INFO Remoting: Remoting now listens on addresses: [akka.tcp://sparkDriver@ea9ec670e429:43346]  
14/12/11 20:33:19 INFO Utils: Successfully started service 'sparkDriver' on port 43346.  
14/12/11 20:33:19 INFO SparkEnv: Registering MapOutputTracker  
14/12/11 20:33:19 INFO SparkEnv: Registering BlockManagerMaster  
14/12/11 20:33:19 INFO DiskBlockManager: Created local directory at /tmp/spark-local-20141211203319-f310  
14/12/11 20:33:19 INFO Utils: Successfully started service 'Connection manager for block manager' on port 58304.  
14/12/11 20:33:19 INFO ConnectionManager: Bound socket to port 58304 with id = ConnectionManagerId(ea9ec670e429,58304)  
14/12/11 20:33:19 INFO MemoryStore: MemoryStore started with capacity 265.4 MB  
14/12/11 20:33:19 INFO BlockManagerMaster: Trying to register BlockManager  
14/12/11 20:33:19 INFO BlockManagerMasterActor: Registering block manager ea9ec670e429:58304 with 265.4 MB RAM, BlockManagerId(&amp;lt;driver&amp;gt;, ea9ec670e429, 58304, 0)  
14/12/11 20:33:19 INFO BlockManagerMaster: Registered BlockManager  
14/12/11 20:33:19 INFO HttpFileServer: HTTP File server directory is /tmp/spark-4c832cee-7ed5-470d-9e41-d4a36227d48f  
14/12/11 20:33:19 INFO HttpServer: Starting HTTP Server  
14/12/11 20:33:19 INFO Utils: Successfully started service 'HTTP file server' on port 55020.  
14/12/11 20:33:19 INFO Utils: Successfully started service 'SparkUI' on port 4040.  
14/12/11 20:33:19 INFO SparkUI: Started SparkUI at http://ea9ec670e429:4040  
14/12/11 20:33:19 WARN NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable  
14/12/11 20:33:19 INFO Executor: Using REPL class URI: http://172.17.0.15:50535  
14/12/11 20:33:19 INFO AkkaUtils: Connecting to HeartbeatReceiver: akka.tcp://sparkDriver@ea9ec670e429:43346/user/HeartbeatReceiver  
14/12/11 20:33:19 INFO SparkILoop: Created spark context..  
Spark context available as sc.

scala>

[/code]

<p style="text-align:justify;">
  Once you reach this <strong>scala </strong>prompt, you're practically done, and you can use your available <strong>SparkContext</strong> (variable <strong>sc</strong>)<strong> </strong>with simple examples :
</p>

[code language=&#8221;scala&#8221;]  
scala> sc.parallelize(1 until 1000).map(_ * 2).filter(_ &lt; 10 ).reduce(_ + _)  
res0: Int = 20  
[/code]

<p style="text-align:justify;">
  If you've got this right, you're all set ! Plus, as this is a Scala prompt, using <tab> you'll have access to all the auto-completion magic a strong type-system can bring you.
</p>

<p style="text-align:justify;">
  So enjoy, take your time and be bold.
</p>