> "The only way to go fast is to go well" - Uncle Bob in [Speed Kills](http://programmer.97things.oreilly.com/wiki/index.php/Speed_Kills)

So why?
=======

* Nice combination of Python/Ruby and Java - concise syntax, static typing and access to Java libraries you already have
* Immutable objects, collections **and API to operate on them.**
   `val smallPrimes: Set[Int] = Set(2, 3) + 5 + 7  // create new set with added 5 and 7. And in Java?`
* Concise code, because brevity is a virtue
    * Inlined/Anonymous functions  `val man = people.find(_.isMan)     // same as people.find(person => person.isMan)`
    * Number of [useful methods](http://www.scala-lang.org/api/current/index.html#scala.collection.immutable.List) in collection to [solve the problem](http://www.scala-lang.org/docu/files/collections-api/collections_3.html)
    * I write types where I want and where I have to as opposed to everywhere (which produces unnecessary code)
* Less errors
    * Concise code
    * Encouraged immutability
    * Code is checked by compiler (static typing)
* Functions and Tuples are first-class citizens
    * Return many values from a function/method!
    * You can define a function inside a function/method, if this is what you need.
* Hybrid of Object Oriented (OOP) and Functional Programming (FP)
    * FP idioms fit data processing better than objects 
      * Spark http://spark.incubator.apache.org/examples.html
      * Scalding https://github.com/twitter/scalding
    * Pattern matching and dynamic dispatch - [why both?](http://www.parleys.com/play/51c1994ae4b0d38b54f4621b/chapter55/about)
    * Polyglot and Poly-paradigm Programming http://polyglotprogramming.com/talks
* I can operate on `BigInteger` using `+` and `*` rather than `add()` or `multiply()`. [Here is an example](http://daily-scala.blogspot.ca/2009/11/bigint-in-scala.html)
* Java interoperability, so you can use Java classes, like `File` or `Date`
* [Easy to start with for Java developers][easy-to-start]
* No more `NullPointerException`! Unless you invoke Java code :P
* Default parameters and named parameters
* Advanced concepts available, when you are ready:
    * Implicit conversions
    * Passed-by-name parameters
    * Actors, Futures
    * Partially applied functions, Currying
    * DSL
* Rod Johnson (creator of Spring framework) joined [Typesafe](http://typesafe.com/company)

> Today’s computing environments are moving towards multicore hardware and cloud computing workloads. Typesafe is strategically positioned to provide innovate solutions with its modern Scala and Akka-based software stack and developer tools for the next wave of applications.
> *Rod Johnson*

What others say?
================

> If I were to pick a language to use today other than Java, it would be Scala.
> [*James Gosling*](http://en.wikipedia.org/wiki/James_Gosling)
 
> I can honestly say if someone had shown me the Programming in Scala book by by Martin Odersky, Lex Spoon & Bill Venners back in 2003 I'd probably have never created Groovy.
> [*James Strachan*](http://macstrac.blogspot.ca/2009/04/scala-as-long-term-replacement-for.html)

* Confessions of a Ruby Developer Whose Heart Was Stolen by Scala http://parleys.com/play/51c178ece4b0d38b54f46217/
* [What is Scala?](http://www.scala-lang.org/what-is-scala.html) by Martin Odersky
* Intel hosts Dr. Martin Odersky presenting Scala 2.10 http://www.youtube.com/watch?v=hiurd7KaSEI
* [Scala Through the Eyes of Java](http://parleys.com/play/5148922b0364bc17fc56c890/)
* [The Why and How of Scala at Twitter](http://www.slideshare.net/al3x/the-how-and-why-of-scala-at-twitter)
* [Working Hard to Keep It Simple](http://www.youtube.com/watch?v=3jg1AheF4n0)
* [Moving from Java to Scala - One year later... (early 2011)](http://java.dzone.com/articles/moving-java-scala-one-year)
* [What has Scala done for you or your organization](https://github.com/mmilewski/whyscala/blob/master/files/WhatHasScalaDoneForYouOrYourOrganization.pdf)
* [Scala is a great mix](http://bharathwrites.in/posts/why-learn-scala/)

Easy to start
=============
[easy-to-start]: #easy-to-start
* You can write Java-style code
* And improve it every day. Read code, write code, ask questions, get excited!
* Immutability is encouraged, but not required
* Some developers could already try [functional programming with Guava](https://code.google.com/p/guava-libraries/wiki/FunctionalExplained)
* [A Brief Intro to Scala](http://www.slideshare.net/tpunder/a-brief-intro-to-scala)
* Very similar to Java, you don't have to start with advanced stuff during the first day.
* Tutorials geared for people coming from... http://docs.scala-lang.org/tutorials/
* [Java to Scala cheatsheet](http://techblog.realestate.com.au/java-to-scala-cheatsheet/) by Ken Scambler
* [Cheatsheet. One huge Scala reference card](http://mbonaci.github.io/scala/)

Better code - [Examples](https://github.com/mmilewski/whyscala/blob/master/Examples.md)
===========

Concise code
------------

* Uses compiler (not developer) to generate boilerplate
    ```scala
    class Person(val name: String)
  
    val p = new Person("Marcin")
    p.name              // Marcin
    p.name = "John"     // name is read-only. If you need write-access, change val to var in class definition
    ```
    Let compiler create getters/setters for you!
    ```scala
    class Person(@BeanProperty val name: String)
  
    val p = new Person("Marcin")
    p.name                 // Marcin
    p.getName              // Marcin
    ```

Fewer bugs
----------
* Wrong placeholders or mismatching number of placeholders
   ```java
   String serviceName = "translator";
   int failures = 4;

   // When formatting String you should use %s, %d and so on
   logger.info(String.format("%s failed %d times.", serviceName, failures));
   
   // But you can also use slf4j's formatter... but the usage is different
   logger.info("{} failed {} times.", serviceName, failures);
   
   // 1. If you mix these two usages, your messages becomes useless.
   // 2. If you provide wrong number of arguments, you are in trouble.
   ```
   ```scala
   // In Scala mechanism for String formatting averts both issues
   val serviceName: String = "translator"
   val failures: Int = 4
   logger.info(s"${serviceName} failed ${failures} times")

   ```

Access to Java libraries is easy
================================

```scala
import java.util.Date
val today = new Date()                  // Brackets are optional
```

Test your ideas
===============
* Online using https://codebrew.io/ or http://www.scalakata.com/
* Locally using Scala REPL http://bcomposes.wordpress.com/2011/08/22/first-steps-in-scala-for-first-time-programmers-part-1/

Where can I learn?
==================
* Scala Tutorials with online REPL http://scalatutorials.com/
* Getting Started With Functional Programming and Scala https://blog.stackmob.com/2013/01/resources-for-getting-started-with-functional-programming-and-scala/
* Twitter's Scala School http://twitter.github.io/scala_school/
* Twitter's Effective Scala http://twitter.github.io/effectivescala/
* Guides and Overviews http://docs.scala-lang.org/overviews/
* [Books On Scala](http://www.scala-lang.org/node/959)
* **Hands-on**
   * Ninety-Nine Scala Problems http://aperiodic.net/phil/scala/s-99/
   * Scala Koans http://www.scalakoans.org/

Other resources
===============
![http://parleys.com/play/5148922b0364bc17fc56c890/chapter60/related](http://s22.postimg.org/r6pkbkxkx/tools_and_scala_features.png)
[Scala Through the Eyes of Java](http://parleys.com/play/5148922b0364bc17fc56c890/chapter60/related)
* [Martin Odersky's Scala With Style](http://www.youtube.com/watch?v=iPitDNUNyR0&t=50s)
* [Scala Days 2013](http://parleys.com/channel/51ae1022e4b01033a7e4b6ca/presentations)
* [Scala & Java 8: Feature Comparison](http://www.infoq.com/articles/java-8-vs-scala)
