---
title: How to remove scaladoc generation from Play 2.2.x Production dist
author: ogirardot
type: post
date: 2014-06-17T16:12:27+00:00

publicize_twitter_user:
  - ogirardot
publicize_twitter_url:
  - http://t.co/IY4Ti1t5fc
publicize_linkedin_url:
  - 'http://www.linkedin.com/updates?discuss=&scope=33725914&stype=M&topic=5884698903277756416&type=U&a=6oCh'
categories:
  - OSS
tags:
  - dist
  - play2
  - Scala

---
<!--more-->
After a few hours of searching through the Play 2 documentation, the play-framework google group and other blogs or sources, i finally found this piece of code that i decided to share with you.

So if, like me, you wanted to remove the Scaladoc generation and packaging inside the <a href="www.playframework.com/documentation/2.2.x/ProductionDist" target="_blank">ProductionDist</a> that you can create from running the **play dist **command, then today's your lucky day.

If you have a **build.sbt** file (and you should) in your Play2 app, then all you need to do is add inside your file **sources in doc in Compile := List() **like that :

[code language=&#8221;scala&#8221;]  
import play.Project._

name := "my-web-project"

playScalaSettings

sources in doc in Compile := List()

libraryDependencies ++= Seq(...)  
[/code]