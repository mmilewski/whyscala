This page collects specific examples to ilustrate that these are invalid:
* "Scala code is unreadable"
* "We don't have time to learn new language"
* "Programming without mutability is hard or not possible"

If you know shorter (or better in any sense) solution, please let me know.

Simple filtering
----------------
```scala
    // scala
    val men = people.filter(_.isMan)
    val adultMen = men.filter(_.age >= 18)
    
```
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
    val grouped: Map[Int, List[People]] = people.groupBy(_.age)
```
```java
   // java
   Give it a try!
```

Power of filtering + sorting + partitioning
--------------------------------------------
Divide a group of students from Canada by top grade and have them sorted by name. [src](http://parleys.com/play/5148922b0364bc17fc56c890/chapter35/about)
```scala
    // scala
    val (topGrades, otherGrades) = studends.filter(_.country == "CA")
                                           .sortBy(_.name)
                                           .partition(_.grade >= 9)
```

```java
    // java
    // ... this is a place for your code ...
```

Check if a string contains any uppercase character
---------------------------------------------------
```scala
    // scala
    val nameHasUpperCase = name.exists(_.isUpperCase) 
```

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

Add default column to the list
------------------------------
```scala
    case class Column(name: String, id: Int)
    
    def addDefaultColumn(cols: Seq[Column], defaultColumn: Column): Seq[Column] = {
      if (cols.exists(_.id == defaultColumn.id)) cols else cols :+ defaultColumn
    }
```
```java
    static class Column {
        String name;
        Integer id;

        public Column(String name, Integer id) {
            this.name = name;
            this.id = id;
        }
    }

    public List<Column> addDefaultColumn(List<Column> cols, Column defaultColumn) {
        boolean present = false;
        for (Column col : cols) {
            if (col.id.equals(defaultColumn.id) {
                present = true;
                break;
            }
        }
        List<Column> result = new ArrayList<Column>(cols);  // No guarantee that `cols` is mutable.
        if (!present) {
            result.add(defaultColumn);
        }
        return result;
    }
```

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

Traversing iterator - print names of first 5 items
------------------------------------------
How is traversing instance of Iterator different than traversing a List? In Java it is...
```scala
   // scala
   Iterator[Item] itor = db.allItems()
   itor.take(5).map(_.name).foreach(println)       // use the same name of methods as for List or Vector
```
```java
   // java
   Iterator<Item> itor = google.Iterators.limit(db.allItems(), 5);    // pain in the neck without Guava though
   Item item;
   while (itor.hasNext() && (item = itor.next())) {     // WAT?
      System.out.println(item.name);
   }

```

Converting collections - has to be simple
-----------------------------------------
```scala
   // scala
   val fruitsList: List[String] = List("orange", "apple", "banana")
   val fruitsSet: Set[String] = Set("orange", "apple", "banana")
   val fruitsArray: Array[String] = fruitsList.toArray
   val fruitsSet2: Set[String] = fruitsArray.toSet               // also fruitsList.toSet
   val backToList: List[String] = fruitsArray.toList
   val fruitsArray: Iterator[String] = fruitsArray.toIterator    // also fruitsList.iterator
   val fruitsStream: Stream[String] = fruitsArray.toStream       // also fruitsList.toStream
```
```java
   // java
   List<String> fruitsList = Arrays.asList("orange", "apple", "banana"); 
   // pardon me, why `Arrays` when I want a list? 
   // Now, how do I create a Set?
   Set<String> fruitSet1 = Arrays.asSet("orange", "apple", "banana");     // ERROR, no such method `asSet`. Eh.
   Set<String> fruitSet2 = new HashSet<String>(Arrays.asList("orange", "apple", "banana"));  // Ok, here we go!
   Set<String> fruitSet3 = Sets.newHashSet("orange", "apple", "banana");  // courtesy of Guava... but still a lot of typing

   // list-array conversion
   String[] fruitsArray = fruitsList.toArray(new String[0]);        // so you say "new String[0]" is easy/intuitive?
   List<String> backToList = fruitsArray.toList();                  // trick not required here, arrays are not generic
   // Iterator<String> fruitsArrayItor = ?? fruitsArray ??;         // how do I create iterator from array?
```

Formatting string
-----------------
```scala
   // In Scala mechanism for String formatting averts both issues
   val serviceName: String = "translator"
   val failures: Int = 4
   logger.info(s"${serviceName} failed ${failures} times")
   
   // To glue few values together
   List("Name", " ", "Lastname").mkString  // "Name Lastname"
   
   // Glue using separator
   List("A", "B", "C", "D").mkString("-->")  // "A-->B-->C-->D"
   
   // Glue using separator and boundaries
   List("A", "B", "C", "D").mkString("{ ", " --> ", " }")  // "{ A --> B --> C --> D }"
```
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
