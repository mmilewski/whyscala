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

Sorting
-------
```scala
    // scala
    val items: List[String] = ...
    val sortedItems = items.sorted    // could it be more obvious?

    // list to sorted set
    val sortedItems = items.to[SortedSet]
    // or
    val sortedItems = SortedSet(items : _*)    // Analogous to Python's foo(*items) mechanism
```
```java
    // java

    // Not so obvious version - I worked it out when wondering what is this FluentIterable thing in Guava API.
    // Where to take instance of Comparator for basic types was also not so obvious.
    List<String> items = ...;  // can be immutable/unmodifiable
    List<String> sortedItems = FluentIterable.from(items).toSortedList(Ordering.natural());

    // More common version, I guess.
    List<String> items = ...;   // can be immutable/unmodifiable, so we have to make a new list
    ArrayList<String> modifiableItems = new ArrayList<String>(items);
    Collections.sort(modifiableItems);
    List<String> sortedItems = modifiableItems;

    // list to sorted set
    SortedSet<String> sortedItems = FluentIterable.from(items).toSortedSet(Ordering.natural());
    // or
    SortedSet<String> sortedItems = new TreeSet(items);

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

Calculate number of instances of each word in a document
--------------------------------------------------------
```scala
// scala
reader.lines()
      .flatMap(_.split(" "))
      .groupBy(identity)
      .mapValues(_.size)
```

```java
// java 8
reader.lines()
      .flatMap(s -> s.splitAsStream(" "))
      .collect(groupingBy(s -> s,
               counting()));
// or
reader.lines()
      .flatMap(s -> s.splitAsStream(" "))
      .collect(groupingBy(s -> s,
               reducing(s -> 1, Integer::sum)));
```
Reference: http://www.parleys.com/play/530758c5e4b06b7f61f3fba8/chapter61/about

Add required column to the list
-------------------------------
```scala
    // scala
    case class Column(name: String, id: Int)
    
    def appendColumnIfNotPresent(cols: Seq[Column], requiredColumn: Column): Seq[Column] = {
      if (cols.exists(_.id == requiredColumn.id)) cols else cols :+ requiredColumn
    }
```
```java
    // java
    static class Column {
        String name;
        Integer id;

        public Column(String name, Integer id) {
            this.name = name;
            this.id = id;
        }
    }

    public List<Column> appendColumnIfNotPresent(List<Column> cols, Column requiredColumn) {
        boolean present = false;
        for (Column col : cols) {
            if (col.id.equals(requiredColumn.id) {
                present = true;
                break;
            }
        }

        if (!present) {
            // No guarantee that `cols` is mutable, so we have to make a copy.
            List<Column> result = new ArrayList<Column>(cols);
            result.add(requiredColumn);
            return Collections.unmodifiableList(result);
            // Alternative version using Guava
            return new ImmutableList.Builder<Column>().addAll(cols).add(requiredColumn).build();
        }
        return Collections.unmodifiableList(result);
    }
```

PoKeMOniZe string
-----------------
Quick stub method for different purposes (e.g. simplest alignment after protocol change).
```scala
    // scala
    def pokemonize(str: String): String = {
        str.map(char => if (Random.nextBoolean()) char.toUpper else char.toLower)
    }

    def pokemonizeAll(strings: Traversable[String]): Traversable[String] = strings.map(pokemonize)

    println(pokemonize("hello hello"))   // e.g. HeLLo woRLD
    println(pokemonizeAll(Seq("hello", "world")))  // e.g. Seq(hELlo, WorlD)
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

     List<String> pokemonizeAll(Collection<String> strings) {
        List<String> result = new ArrayList<String>();
        for (String s : strings) {
            result.add(pokemonize(result));
        }
        return result;
    }
```
### Pokemonize part of string
The other day you would like to consider only part of input string as input. It would be 
```scala
    // scala
    def pokemonizeLastPart(str: String): String = pokemonize(str.split('.').last)
```
```java
    // java
    public String pokemonizeLastPart(String str) {
        String[] parts = str.split("\\.");
        return parts[parts.length-1];
    }

    // alternative
    public String pokemonizeLastPart(String str) {
        return Iterables.getLast(Arrays.asList(str.split("\\.")));
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

   // Those methods are added for your convenience. General mechanism looks like this:
   val fruitsSet: Set[String] = fruitsArray.to[Set]
   val fruitsArray: Array[String] = fruitsArray.to[Array]
   // this flexibility allows you to create any collection!
   val fruitsBuffer: Buffer[String] = fruitsArray.to[ListBuffer]
   val fruitsSortedSet: SortedSet[String] = fruitsArray.to[SortedSet]
   val fruitsDLList: DoubleLinkedList[String] = fruitsArray.to[DoubleLinkedList]

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
   // Iterator<String> fruitsArrayItor = Arrays.asList(fruitsArray).iterator;
```
### Convert list of pairs to map
```scala
   // scala
   val translateNum: Map[Int, String] = List((1, "one"), (2, "two"), (10, "ten")).toMap
```
Well, we don't event have pairs in Java. However, for list of objects it would be
```java
   // java
   Map<Integer, String> translateNum = Maps.newHashMap();      // Gauva library
   for (MagicObject obj: objects) {
       translateNum.put(obj.getNumber(), obj.getTranslation());
   }
```
Scala version of such transformation would be
```scala
    // scala
    val translateNum: Map[Int, String] = objects.map(obj => (obj.getNumber(), obj.getTranslation())).toMap
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
