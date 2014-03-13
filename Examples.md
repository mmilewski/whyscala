This page collects some example which may be useful for arguments like
* "We don't have time to learn new language"
* "Scala code is unreadable"

If you know shorter (or better in any sense) solution, please let me know.

Pokemonize string
-----------------
```java
    String pokemonize(String str) {
        StringBuilder sb = new StringBuilder();
        Random rnd = new Random();
        for (int i = 0; i < str.length(); i++) {
            char c = Character.toLowerCase(str.charAt(i));
            if (rnd.nextBoolean()) {
                c = Character.toUpperCase(str.charAt(i));
            }
            sb.append(c);
        }
        return sb.toString();
    }

    Collection<String> pokemonize(Collection<String> strings) {
        List<String> result = new ArrayList<String>();
        for (String s : strings) {
            result.add(pokemonize(result));
        }
        return result;
    }

```

```scala
  def pokemonize(str: String): String = str.map(char => if (Random.nextBoolean()) char.toUpper else char.toLower)
  def pokemonize(strings: Traversable[String]): Traversable[String] = strings.map(pokemonize)
```
