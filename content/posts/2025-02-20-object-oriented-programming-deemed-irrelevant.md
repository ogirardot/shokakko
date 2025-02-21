---
title: Object oriented programming deemed irrelevant
author: ogirardot
type: post
date: 2025-02-20T09:40:00+00:00
timeline_notification:
  - 1740044419
wpcom_is_first_post:
  - 1
firehose_sent:
  - 1740044413
categories:
  - Java
  - OSS
  - TechZone
tags:
  - dev
  - Python

---
<p style="text-align: justify">
  I've been coding since 2006, during this time I've seen multiple trends & technologies emerge, rise and fall - nowadays the elephant in the room is the bad press around OOP languages the likes of Java, C#, C++.
</p>
<!--more-->

<p style="text-align: justify">
  Our profession is no stranger to this kinda of feud and debate, for example, at the start of my career I learned and was told to quickly forget Remote Procedure Calls technologies like <a href="https://fr.wikipedia.org/wiki/Common_Object_Request_Broker_Architecture">CORBA</a>, SOAP with the bulk of what we called Web Services at the time - (spoiler: it came back with gRPC useful things tend to come back).
</p>

<p style="text-align: justify">
  I was only told at the time that my job as a software engineer was going to be irrelevant soon enough because of MDA - <a href="https://en.wikipedia.org/wiki/Model-driven_architecture">Model Driven Architecture</a> - and if I wanted to really build things my goal should be to harness all the UML/Merise diagram types perfectly and then feed them all to <a href="https://projects.eclipse.org/projects/modeling.emf.emf">Eclipse EMF</a> (still alive btw) for it to generate the code (like a good engineer should because *really doing things* is kinda dirty anyway).
</p>

## OOP and programming languages {.wp-block-heading}

<p style="text-align: justify">
  One thing that was a given at the time was the clear win of the Object Oriented programming languages for &#8220;serious work&#8221; - C was already considered too low level - so the clear go-to languages built from the ground up with OOP in mind were Java, C# and C++.
</p>

<p style="text-align: justify">
  All the other languages wanted in on the action and added some concepts of Classes afterwards some in a clunky limited way like PHP and some with more attention to details like Python.
</p>

## Fast forward to now {.wp-block-heading}

<p style="text-align: justify">
  Nowadays OOP is the bad guy, the one responsible for all the evils in this world (along with Waterfall, Agile, Web Services and Design Patterns). To be clear it is denigrated in the tech news and as a general consensus in the ecosystem considered <a href="https://medium.com/@jacobfriedman/object-oriented-programming-is-an-expensive-disaster-which-must-end-2cbf3ea4f89d">too expensive</a>, <a href="https://news.ycombinator.com/item?id=18526490">too bloated</a> (special mention for the epic <a href="https://dpc.pw/posts/the-faster-you-unlearn-oop-the-better-for-you-and-your-software">The faster you unlearn OOP, the better for you and your software</a>) and a waste of precious time creating more problems than it solves, especially with the challenges we face nowadays (efficient multicore usage, async/data-intensive applications, deep integration with Machine Learning and distributed systems to mention a few).
</p>

<p style="text-align: justify">
  Ok it looks like a grim picture, let's take a step back and look at what people actually do, and if we take a look at the <a href="https://survey.stackoverflow.co/2023/">Stack Overflow developer survey of 2023</a>, it checks out, the most popular and widely used programming languages today are not object oriented from the ground up - they are all scripting languages (except SQL) :
</p><figure class="wp-block-image size-large">

[<img decoding="async" src="https://ogirardot.wordpress.com/wp-content/uploads/2025/02/image-1.png?w=1024" alt="" class="wp-image-1311" />][1]</figure> 

The same conclusion can be drawn if we take at the [Stack Overflow developer survey of 2024][2] : <figure class="wp-block-image size-large">

[<img decoding="async" src="https://ogirardot.wordpress.com/wp-content/uploads/2025/02/image.png?w=1024" alt="" class="wp-image-1309" />][3]</figure> 

<p style="text-align: justify">
  However if we take a look at the number of years of experience of the respondents we can see some biais in the datasets the bulk of respondents being <10 years experience on the job while according to <a href="https://datausa.io/profile/soc/software-developers?employment-measures=workforceEOT">DataUSA</a> the average age in the industry is as of 2022 <strong>39 years old</strong>
</p><figure class="wp-block-image size-large">

[<img decoding="async" src="https://ogirardot.wordpress.com/wp-content/uploads/2025/02/image-2.png?w=1024" alt="" class="wp-image-1316" />][4]</figure> 



<p style="text-align: justify">
  And it's easy to see this self-fulfilling prophecy in action with the majority of bootcamps and influencers supposed to help you get a 6 figure job in tech in 4 days tell you that learning JavaScript is the best way to become a FullStack Engineer ever <em>because you can use it on both the frontend and the backend</em> (🤯 !) and Python the best way to get into DataScience <em>(hard to argue about this one...)</em>.
</p>

<p style="text-align: justify">
  So yes, the lingua franca of new developers is now closer in terms of paradigm to a (mostly) dynamically typed - imperative style of programming and it seems, as a whole, experienced developers stayed loyal (<em>PyCon or Devoxx conferences still see ~5000 participants per day each year</em> <em>and JavaOne (rebranded DevNexus) in the USA sees ~10k participants per day</em>) or moved *laterally* :
</p>

<ul class="wp-block-list">
  <li>
    some experienced developers in OOP languages have moved on to functional programming languages to overcome some of the trauma they faced
  </li>
  <li>
    others moved on from strict Java / C# etc... to Kotlin/Scala or other more modern forms of the language while Java integrated some of these features to stay relevant and dominant (Streams, lambda, default implementation etc...)
  </li>
</ul>

<p style="text-align: justify">
  Finally the emergence of a new brand of lower level languages like Go and Rust means that even some of the newcomers had additional options to shield themselves from the &#8220;enterprise languages&#8221;.
</p>

## Where to go from there {.wp-block-heading}

<p style="text-align: justify">
  There now seems to be a schism between older generations of programmers and newer generations, the later disregarding for the main part all the teachings (the bad and the good) that object oriented programming brought to the table.
</p>

<p style="text-align: justify">
  Now let's be frank, none of the concepts that OOP pushed for are special to these languages especially in the later years :
</p>

<ul class="wp-block-list">
  <li>
    the simple fact of defining Abstractions (<em>not too much, not too little</em>) and following the <a href="https://en.wikipedia.org/wiki/Dependency_inversion_principle">Dependency Inversion Principle</a>
  </li>
  <li>
    the <a href="https://en.wikipedia.org/wiki/Single-responsibility_principle">single responsibility principle</a>
  </li>
  <li>
    the <a href="https://en.wikipedia.org/wiki/Encapsulation_(computer_programming)">encapsulation</a> habit
  </li>
  <li>
    the <a href="https://en.wikipedia.org/wiki/Composition_over_inheritance">composition over inheritance</a> principle
  </li>
</ul>

<p style="text-align: justify">
  Or as we now say broadly following the <a href="https://en.wikipedia.org/wiki/SOLID">SOLID</a> principles, none of these concepts are things that you can only do with classes, inheritance or a stubbornly opinionated Object Oriented programming language.
</p>

<p style="text-align: justify">
  As a side note, most of the time it's easier to follow the spirit of these principles in a functional programming language, but amongst the list of popular programming languages they are notoriously absent, the closer we'd get is that some of these languages have &#8220;functional programming features&#8221; like first-order functions, map, filters and that's it.
</p>

<p style="text-align: justify">
  OOP has <a href="https://loup-vaillant.fr/articles/deaths-of-oop">died many times in the past</a> it has however survived until today but in other forms. We still call this by continuity OOP each time mostly because OOP has always been very loosely defined - the projects we build today using OOP languages and frameworks do not use as many abstractions, layers of indirections, overrides or even overloads than when the hype was at its peak, and it's a good thing! For simplicity is always a good thing!
</p>

<p style="text-align: justify">
  This lack of definition is even clearer if we go back to Alan Kay the creator of SmallTalk who coined the term &#8220;Object Oriented Programming&#8221; when he meant to say the following :
</p>

<blockquote class="wp-block-quote is-layout-flow wp-block-quote-is-layout-flow">
  <p style="text-align: justify" id="ecea">
    <strong><em>“OOP to me means only messaging, local retention and protection and hiding of state-process, and extreme late-binding of all things.”</em></strong>
  </p>
</blockquote>

<p style="text-align: justify">
  None of the current leaders in OOP are message-passing oriented (sadly), yet we consider them object oriented.
</p>

<p style="text-align: justify">
  I do not care that much for the survival of OOP but I do see the value in the core teachings and the values in terms of separation of concerns it brought us - the efficient tooling and compilers that have been developed and refined for the last 30 years. We, as a profession, are not doomed to repeat the cycle of hype, fame, banishment and rewrite, I've already experienced multiple times in my short career.
</p>

<p style="text-align: justify">
  We should encourage all software engineers to strive for knowledge, learn, and develop critical thinking rather than forget all kinds of rational behavior, considering only the hype and prejudice of our times - in the end, even &#8220;old&#8221; programming languages and paradigms can be <a href="https://medium.com/nerd-for-tech/is-oop-relevant-today-3b3fdc2d1ab2#:~:text=Wrapping%20Up-,Is%20OOP%20still%20an%20effective%20software%20development%20tool%20or%20is,and%20communications%20models%20are%20crucial.">relevant</a> today for the objectives we all have; to stay sane in a convoluted codebase.
</p>

 [1]: https://ogirardot.wordpress.com/wp-content/uploads/2025/02/image-1.png
 [2]: https://survey.stackoverflow.co/2024/technology#most-popular-technologies-language
 [3]: https://ogirardot.wordpress.com/wp-content/uploads/2025/02/image.png
 [4]: https://ogirardot.wordpress.com/wp-content/uploads/2025/02/image-2.png