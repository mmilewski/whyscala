Tips and Practices
==================
* public interface should be super clear
* separate creation (factory, builder, provider, wiring objects) from use (business logic, domain abstraction)
* context, data, algorithm, processor/executor
* declarative is more readable than imperative

> "The only way to go fast is to go well" - *Uncle Bob*

* "When in doubt, leave it out" http://www.infoq.com/articles/API-Design-Joshua-Bloch
* Pragmatic Programmer tips http://pragprog.com/the-pragmatic-programmer/extracts/tips
* Things Every Programmer Should Know http://www.javacodegeeks.com/2010/12/things-every-programmer-should-know.html

Architecture
============

Philosophy
* Optimize for **deletability** http://vimeo.com/108441214 by Greg Young
   * You cannot create the first model right and making a change in model that does not work is hard (and error prone). So if you could rewrite the module from scratch quickly, you can have a better model and solve problem more easily.
   * Microservices, Actors, SOA, Objects,... are all the same concept. Read Alan Key who said about "small computers exchanging messages".
   * Small programs are not a new concept, see Linux - you can rewrite ls, grep, cp and others in a week or two.
   * Reading big, interconnected codebases takes weeks, months, or years. Hence it takes a lot of time for a new person to start contributing.
   * SOA in Dutch means what STD means in English.

Design Patterns
* Design patterns Stories http://www.programcreek.com/java-design-patterns-in-stories/
* Design patterns in real life http://www.codeproject.com/Articles/29036/Patterns-in-Real-Life
* Examples of design patterns http://stackoverflow.com/questions/1673841/examples-of-gof-design-patterns
 
* The Architecture of Open Source Applications http://aosabook.org/en/index.html
  * ZeroMQ http://aosabook.org/en/zeromq.html

* Designing and Deploying Internet-Scale Services https://www.usenix.org/legacy/events/lisa07/tech/full_papers/hamilton/hamilton_html/ (James Hamilton)

Testing
=======
* OO Design for Testability http://www.youtube.com/watch?v=acjvKJiOvXw (Miško Hevery, 2009)
    * new operators inside object,
    * global state, 
    * Law of Demeter violation
* The Deep Synergy Between Testability and Good Design http://vimeo.com/15007792 (Michael Feathers, 2010)
    * Testing is Easy in the Presence of Good Design - testing issues explained as design issues
