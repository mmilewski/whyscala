So why?
=======
> Any fool can write code that a computer can understand. Good programmers write code that humans can understand.  
> *Martin Fowler*

* Static typing - is a blessing not a curse
* Immutable collections **and API to operate on them.**
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
* Default parameters and named parameters
* Hybrid of object oriented and functional programming
    * Pattern matching and dynamic dispatch - [why both?](http://www.parleys.com/play/51c1994ae4b0d38b54f4621b/chapter55/about)
* I can operate on `BigInteger` using `+` and `*` rather than `add()` or `multiply()`. [Here is an example](http://daily-scala.blogspot.ca/2009/11/bigint-in-scala.html)
* Java interoperability, so you can use Java classes, like `File` or `Date`
* [Easy to start with for Java developers][easy-to-start]
* No more `NullPointerException`! Unless you invoke Java code :P
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

Easy to start
=============
[easy-to-start]: #easy-to-start
* You can write Java-style code, and then improve it every day (this is not true for e.g. Haskell)
* Immutability is encouraged, but not required
* Some developers could already try [functional programming with Guava](https://code.google.com/p/guava-libraries/wiki/FunctionalExplained)
* [A Brief Intro to Scala](http://www.slideshare.net/tpunder/a-brief-intro-to-scala)
* Very similar to Java, you don't have to start with advanced stuff during the first day.
* Tutorials geared for people coming from... http://docs.scala-lang.org/tutorials/
* [Java to Scala cheatsheet](http://techblog.realestate.com.au/java-to-scala-cheatsheet/) by Ken Scambler

Better code
===========

Concise code
------------

* Contains only the essential parts. If you need to filter a collection, you should be asked only for a predicate.
    ```java
    // java
    List<Person> men = Lists.newLinkedList()    // courtesy of Guava library. Without that even more unnecessary code
    for(Person p: people) {
        if (p.isMan()) {
            men.add(p);
        }
    }
    // and what if I want to have both `men` and `adult men`? 

    // scala
    val men = people.filter(_.isMan)
    val adultMen = men.filter(_.age >= 18)
    
    // have you ever tried partitioning the collection in Java? In Scala...
    val (underage, adult) = people.partition(_.age < 18)
    ```

* Uses anonymous functions to get the job done
    ```scala
    case class Person(val name:String, val age:Int, val isMan: Boolean)
    val people = List(
         Person("Marcin", 20, true),     // `new` is not required becaue Person is a `case class`, not just a `class` 
         Person("Dorota", 10, false), 
         Person("Peter", 16, true))
    
    // for each man with name "Marcin" or "Peter" print his name and age
    val goodNames = Set("Marcin", "Peter")
    people.filter(person => person.isMan && goodNames.contains(person.name)).foreach{ person =>
        println("${person.name} is ${person.age} years old.") 
    }
    ```
    So you say it wouldn't take a lot more code in Java, eh? What if I want to do further processing on this people? In Scala, I just extract variable `folks`
    ```scala
    // for each man with name "Marcin" or "Peter" print his name and age
    val goodNames = Set("Marcin", "Peter")
    val folks = people.filter(person => person.isMan && goodNames.contains(person.name))
    folks.foreach { person =>
        println("${person.name} is ${person.age} years old.") 
    }
    folks.filter(_.age > 18).foreach(giveBeer(_))
    ```
    
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

* Creating a collection & converting one collection to the other is easy - **it have to be**, you do this many times a day!
   ```java
   // java
   List<String> fruitsList = Arrays.asList("orange", "apple", "banana"); 
   // pardon me, why `Arrays` when I want a list? Ok, let it be, but how do I create a Set?
   Set<String> fruitSet1 = Arrays.asSet("orange", "apple", "banana");     // ERROR, no such method `asSet`
   Set<String> fruitSet2 = new HashSet<String>(Arrays.asList("orange", "apple", "banana"));  // ok, here we go!
   Set<String> fruitSet3 = Sets.newHashSet("orange", "apple", "banana");  // courtesy of Guava... but still a lot of typing

   String[] fruitsArray = fruitsList.toArray(new String[0]);              // so you say "new String[0]" is easy/intuitive?
   List<String> backToList = fruitsArray.toList();                        // trick not required here, arrays are not generic
   // Iterator<String> fruitsArrayItor = ?? fruitsArray ??;      // how do I do that?
   
   // scala
   val fruitsList: List[String] = List("orange", "apple", "banana")
   val fruitsSet: Set[String] = Set("orange", "apple", "banana")
   val fruitsArray: Array[String] = fruitsList.toArray
   val fruitsSet2: Set[String] = fruitsArray.toSet               // also fruitsList.toSet
   val backToList: List[String] = fruitsArray.toList
   val fruitsArray: Iterator[String] = fruitsArray.toIterator    // also fruitsList.iterator
   val fruitsStream: Stream[String] = fruitsArray.toStream       // also fruitsList.toStream
   ```

* How is traversing instance of Iterator different than a List? In Java it is...
   ```java
   // How do you print names of first 5 items?

   // java
   Iterator<Item> itor = google.Iterators.limit(db.allItems(), 5);    // pain in the neck without Guava though
   Item item;
   while (itor.hasNext() && (item = itor.next())) {     // WAT?
      System.out.println(item.name);
   }

   // scala
   Iterator[Item] itor = db.allItems()
   itor.take(5).map(_.name).foreach(println)       // use the same name of methods as for List or Vector
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

Getting things done
-------------------

* Divide a group of students from Canada by top grade and have them sorted by name. [src](http://parleys.com/play/5148922b0364bc17fc56c890/chapter35/about)

    ```java
    // java
    
    //
    // ... this is a place for your code ...
    //
    ```
    
    
    ```scala
    // scala
    val (topGrades, otherGrades) = studends.filter(_.country == "CA")
                                           .sortBy(_.name)
                                           .partition(_.grade >= 9)
    ```


* Check if a string contains any uppercase character.
    ```java
    // java
    boolean nameHasUpperCase = false;
    for (int i = 0; i < name.length(); ++i) { 
        if (Character.isUpperCase(name.charAt(i))) { 
            nameHasUpperCase = true; 
            break; 
        }    
    }
    ```

    ```scala
    // scala
    val nameHasUpperCase = name.exists(_.isUpperCase) 
    ```


Access to Java libraries is easy
================================

```scala
import java.util.Date
val today = new Date()                  // Brackets are optional
```

Test your ideas
===============
* Online using Scala Kata http://www.scalakata.com/
* Online using Online Compiler http://www.compileonline.com/compile_scala_online.php
* Locally using Scala REPL http://bcomposes.wordpress.com/2011/08/22/first-steps-in-scala-for-first-time-programmers-part-1/

Where can I learn?
==================
* Scala Tutorials with online REPL http://scalatutorials.com/
* Getting Started With Functional Programming and Scala https://blog.stackmob.com/2013/01/resources-for-getting-started-with-functional-programming-and-scala/
* Twitter's Scala School http://twitter.github.io/scala_school/
* Twitter's Effective Scala http://twitter.github.io/effectivescala/
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
