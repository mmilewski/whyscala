This page collects specifig examples against:
* "Scala code is unreadable"
* "We don't have time to learn new language"

If you know shorter (or better in any sense) solution, please let me know.

PoKeMOniZe string
-----------------
Quick stub method for different purposes (e.g. simplest alignment after protocol change).
```scala
  // scala
  def pokemonize(str: String): String = str.map(char => if (Random.nextBoolean()) char.toUpper else char.toLower)
  def pokemonize(strings: Traversable[String]): Traversable[String] = strings.map(pokemonize)
```
```java
    // java
    String pokemonize(String str) {
        StringBuilder sb = new StringBuilder();
        Random rnd = new Random();
        for (int i = 0; i < str.length(); i++) {
            sb.append(
                rnd.nextBoolean()
                    ? Character.toUpperCase(str.charAt(i))
                    : Character.toLowerCase(str.charAt(i))
            );
        }
        return sb.toString();
    }

     List<String> pokemonize(Collection<String> strings) {
        List<String> result = new ArrayList<String>();
        for (String s : strings) {
            result.add(pokemonize(result));
        }
        return result;
    }
```

Simple filtering
----------------
```java
    // java
    List<Person> men = Lists.newArrayList()   // courtesy of Guava library
    for(Person p: people) {
        if (p.isMan()) {
            men.add(p);
        }
    }
    // and what if I want to have both `men` and `adult men`? Would you write another loop or (tightly) cuple them?
    // I say tightly, because in other situation you may want only one of them, so I guess it is better to keep
    // them separate.
```
```scala
    // scala
    val men = people.filter(_.isMan)
    val adultMen = men.filter(_.age >= 18)
    
```
Another example + string interpolation
```scala
    case class Person(val name:String, val age:Int, val isMan: Boolean)
    val people = List(
         Person("Marcin", 20, true),  // `new` is not required becaue Person is a `case class`, not just a `class` 
         Person("Dorota", 10, false), 
         Person("Peter", 16, true))
    
    // for each man with name "Marcin" or "Peter" print his name and age
    val goodNames = Set("Marcin", "Peter")
    people.filter(person => person.isMan && goodNames.contains(person.name)).foreach{ person =>
        println(s"${person.name} is ${person.age} years old.") 
    }
```
So you say it wouldn't take a lot more code in Java, eh? What if I want to do further processing on these filtered people? In Scala, I just extract variable `folks`
```scala
    // for each man with name "Marcin" or "Peter" print his name and age
    val goodNames = Set("Marcin", "Peter")
    val folks = people.filter(person => person.isMan && goodNames.contains(person.name))
    folks.foreach { person =>
        println(s"${person.name} is ${person.age} years old.") 
    }
    folks.filter(_.age > 18).foreach(giveBeer(_))
```

Partitioning, Grouping
----------------------
```scala
    // scala
    // when there are only two groups described by a predicate
    val (underage, adult) = people.partition(_.age < 18)
    
    // when you need grouping just...
    val grouped: [Int, List[People]] = people.groupBy(_.age)
```
```java
   // java
   Give it a try!
```

