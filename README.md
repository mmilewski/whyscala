> Any fool can write code that a computer can understand.  Good programmers write code that humans can understand.  
> *Martin Fowler*

So why?
=======

* Static typing - is a blessing not a curse
* Concise code
    * Inlined/Anonymous functions  `val man = people.find(_.isMan)     // same as people.find(person => person.isMan)`
    * Number of [useful methods](http://www.scala-lang.org/api/current/index.html#scala.collection.immutable.List) in collection
    * I write types where I want and where I have to as opposed to everywhere, which produces long code
* Less errors
    * concise code (see above)
    * Encouraged immutability
* Functions and Tuples are first-class citizens
* Hybrid of object oriented and functinal programming
* Pattern matching
* I can operate on `BigInteger` using + and * rather than add() or multiply(). [See here](http://daily-scala.blogspot.ca/2009/11/bigint-in-scala.html)
* Default parameters and named parameters
* I can define auxiliary function inside a method
* Easy to understand by Java developers.
    * You can write Java-style code, and then improve it every day (this is not true for e.g. Haskell)
    * Immutability is encouraged, but not required
    * Some developers could already try [functional programming with Guava](https://code.google.com/p/guava-libraries/wiki/FunctionalExplained)
* Access to Java classes, like `File` or `Date`
* Has advanced concepts, when you are ready
    * Implicit conversions
    * Passed-by-name parameters
    * Actors
    * Partially applied functions, Currying
* [A Brief Intro to Scala](http://www.slideshare.net/tpunder/a-brief-intro-to-scala)


Easy to start
=============
* Very similar to Java, you don't have to start with advanced stuff during the first day.
* Tutorials geared for people coming from... http://docs.scala-lang.org/tutorials/

Better code
===========

Concise code... 
---------------

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
    
    // have you ever tried partitioning the collection in Java? No? Have fun, while I have my job done:
    val (underage, adult) = people.partition(_.age < 18)
    ```

* Uses inlined functions to get the job done
    ```scala
    case class Person(val name:String, val age:Int, val isMan: Boolean)
    val people = List(new Person("Marcin", 20, true), new Person("Dorota", 10, false), new Person("Peter", 16, true))
    
    // for each man with name "Marcin" or "Peter" print his name and age
    val goodNames = Set("Marcin", "Peter")
    people.filter(person => person.isMan && goodNames.contains(person.name)).foreach{ person =>
        println( "%s is %d years old.".format(person.name, person.age) ) 
    }
    ```
    So you say it wouldn't take a lot more code in Java, eh? What if I want to do something else with guys I found? How would your Java code adapt to new requirements? In Scala, I just extract variable `folks`
    ```scala
    // for each man with name "Marcin" or "Peter" print his name and age
    val goodNames = Set("Marcin", "Peter")
    val folks = people.filter(person => person.isMan && goodNames.contains(person.name))
    folks.foreach{ person =>
        println( "%s is %d years old.".format(person.name, person.age) ) 
    }
    goForABearWith(folks)    
    ```
    
* Uses collection's methods to get the job done and avoid errors.
    ```java
    // java
    boolean nameHasUpperCase = false;
    for (int i = 0; i < name.length(); ++i) { 
        if (Character.isUpperCase(name.charAt(i))) { 
            nameHasUpperCase = true; 
            break; 
        }    
    }

    // scala
    val nameHasUpperCase = name.exists(_.isUpperCase) 
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


Access to Java libraries
========================

```scala
import java.util.Date
val today = new Date()                  // Brackets are optionall
```

Test your ideas
===============
* Online using Scala Kata http://www.scalakata.com/
* Locally using Scala REPL http://bcomposes.wordpress.com/2011/08/22/first-steps-in-scala-for-first-time-programmers-part-1/


What other say?
================
* Confessions of a Ruby Developer Whose Heart Was Stolen by Scala http://parleys.com/play/51c178ece4b0d38b54f46217/
* Intel hosts Dr. Martin Odersky presenting Scala 2.10 http://www.youtube.com/watch?v=hiurd7KaSEI


Where can I learn?
==================
* No matter how much you read, if you don't start writing programs, it is not gonna work.
* Twitter's Scala School http://twitter.github.io/scala_school/
* Twitter's Effective Scala http://twitter.github.io/effectivescala/
* Martin Odersky's Scala With Style http://www.youtube.com/watch?v=iPitDNUNyR0&t=50s
