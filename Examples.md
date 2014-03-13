This page collects specifig examples against:
* "Scala code is unreadable"
* "We don't have time to learn new language"

If you know shorter (or better in any sense) solution, please let me know.

PoKeMOniZe string
-----------------
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
