---
title: Dagger and Play 2 Java
author: ogirardot
type: post
date: 2014-07-28T16:29:42+00:00

publicize_twitter_url:
  - http://t.co/T9mXky3IW1
publicize_twitter_user:
  - ogirardot
publicize_linkedin_url:
  - 'http://www.linkedin.com/updates?discuss=&scope=33725914&stype=M&topic=5899561138638594048&type=U&a=vxfo'
categories:
  - Java
  - OSS
  - Scala

---
<!--more-->
<p style="text-align:justify;">
  I recently got the occasion of trying out Play 2 in Java and i must say the Play 2 Framwork looks actually really good in Java too.
</p>

<p style="text-align:justify;">
  But, of course... there is a but, one of the few things that strikes you first, and i must say with great intensity, is the <em>mandatory</em> static methods that you must put in your <strong>Controllers </strong>in order to define your routes. Exemple :
</p>

[code language=&#8221;java&#8221;]  
// in app/controllers/Application.java  
package controllers;

import play.mvc.Controller;  
import play.mvc.Result;  
import service.CoffeeService;  
import views.html.index;

public class Application extends Controller {

public static Result index() {  
return ok(index.render("Your application is ready."));  
}

}  
[/code]

<p style="text-align:justify;">
  And with the routes defined as such :
</p>

[code]  
\# Home page  
GET / controllers.Application.index()

[/code]

<p style="text-align:justify;">
  This is relatively great... if you like starting off with the wrong foot. I won't talk about <strong>modularization</strong> or the <strong>danger of spaghetti-code</strong>, neither will i argue that this is not great for testing <strong>controllers</strong> that will use <strong>services </strong>or any other kind of <b>external dependencies.</b>
</p>

<p style="text-align:justify;">
  Luckily, the <strong>Play 2 Framwork </strong> people have thought long and hard when they designed their systems, and if they won't force you to use any kind of dependency injection systems, they'll allow you to plugin-in your preffered choice. This is clearly <a title="Dependency Injection in Scala with Play2 " href="http://www.playframework.com/documentation/latest/ScalaDependencyInjection" target="_blank">documented here</a> but this is in Scala and you might think it's not available for <strong>Play 2 Java</strong>, and you would be wrong.
</p>

<p style="text-align:justify;">
  So here's a little example on how to do it with a really great project by the teams at <a title="Square" href="http://squareup.com" target="_blank">Square</a> called <a href="http://square.github.io/dagger/" target="_blank">Dagger</a>. Dagger relies on the annotation processing framework of Java to be able to plug itself as an extra step of the compiler and try, as much as possible, to do dependency injection checks (and maybe more) at compile-time. So let's try to use it in a simple Java app :
</p>

[code]  
// in build.sbt - we'll add the dependency  
name := "app"

version := "1.0-SNAPSHOT"

libraryDependencies ++= Seq(  
javaJdbc,  
javaEbean,  
cache,  
"com.squareup.dagger" % "dagger" % "1.2.2",  
"com.squareup.dagger" % "dagger-compiler" % "1.2.2"  
)

play.Project.playJavaSettings  
[/code]

[code language=&#8221;java&#8221;]  
// in app/controllers/Application.java - we'll inject a simple Service via dagger  
package controllers;

import play.mvc.Controller;  
import play.mvc.Result;  
import service.CoffeeService;  
import views.html.index;

import javax.inject.Inject;

public class Application extends Controller {

private CoffeeService coffeeService;

@Inject  
public Application(CoffeeService service) {  
coffeeService = service;  
}

public Result index() {  
return ok(index.render("Your application " + this.toString() +" is ready. " + coffeeService.toString()));  
}

}  
[/code]

<p style="text-align:justify;">
  Finally to make it all work we need to change the routes file and override the &#8220;Global&#8221; configuration class :
</p>

[code language=&#8221;java&#8221;]  
// in app/Global.java - we'll create this class and override the controller instance creation  
import dagger.ObjectGraph;  
import module.ProductionModule;  
import play.Application;  
import play.GlobalSettings;

public class Global extends GlobalSettings {

private ObjectGraph objectGraph;

@Override  
public void beforeStart(Application app) {  
super.beforeStart(app);  
objectGraph = ObjectGraph.create(new ProductionModule());  
}

@Override  
public <A> A getControllerInstance(Class<A> controllerClass) throws Exception {  
return objectGraph.get(controllerClass);  
}  
}  
[/code]

<p style="text-align:justify;">
  and
</p>

[code]  
\# Home page  
GET / @controllers.Application.index()  
[/code]

<p style="text-align:justify;">
  The <strong>@controllers.Application.index() </strong>tells the whole system that now he has to create a new instance of Application controller and he will get the controller's instance through the overriden method in Global.
</p>

<p style="text-align:justify;">
  The goal was not in this article to teach you how to use Dagger or Play, rather more to show you how the two of them can work together. If you want to see the whole project, it's available online on <a href="https://github.com/lateralthoughts/dagger-play-di-example" target="_blank">https://github.com/lateralthoughts/dagger-play-di-example</a>. So if you want to know more, clone the project and play with it. Any feedback would be appreciated.
</p>

<p style="text-align:justify;">
  <em>Vale</em>
</p>