Easy to start
=============
* Very similar to Java, you don't have to start with advanced stuff during the first day.
* Tutorials geared for people coming from... http://docs.scala-lang.org/tutorials/

Better code
===========

> Any fool can write code that a computer can understand.  Good programmers write code that humans can understand.  
> *Martin Fowler*


Concise code... 
---------------
* Is easier to read 
  ```scala
  val (underage, adult) = people.partition(_.age <= 18)
  
  val adultMen = adult.filter(_.isMan)

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
