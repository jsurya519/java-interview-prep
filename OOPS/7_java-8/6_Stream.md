## Stream:
1. A Stream is a sequence of elements that supports functional-style operations.
2. Stream does not store data. It only processes data.
3. **Lazy Execution:** Operations are executed only when needed (terminal operation).

![72254b0a13613873e0d1658514d3f307.png](:/e37a7c02195d4efbb9ba6da64ab4282e)

## Internal Working of Stream:
**1. Create a Stream:** From collections, arrays or static methods.
**2. Apply Intermediate Operations:** Transform data (e.g., filter(), map(), sorted()).
**3. Apply Terminal Operation:** Produce a result (e.g., forEach(), collect(), reduce()).

## Creation of Stream:
There are four ways of creating a Stream
**1. From a Collection:** Create a stream directly from a List, Set or any Collection using stream()
**2. From an Array:** Use Arrays.stream(array) to convert an array into a stream.
**3. Using Stream.of():** Create a stream from a fixed set of values using Stream.of().
**4. Infinite Stream:** Generate an unbounded sequence using Stream.iterate() or Stream.generate()

```java
import java.util.*;
import java.util.stream.*;

public class StreamCreation {
    public static void main(String[] args) {
        // 1. From a Collection
        List<String> list = Arrays.asList("Java", "Python", "C++");
        Stream<String> stream1 = list.stream();

        // 2. From an Array
        String[] arr = {"A", "B", "C"};
        Stream<String> stream2 = Arrays.stream(arr);

        // 3. Using Stream.of()
        Stream<Integer> stream3 = Stream.of(1, 2, 3, 4, 5);

        // 4. Infinite Stream (limit to avoid infinite loop)
        Stream<Integer> stream4 = Stream.iterate(1, n -> n + 1).limit(5);
        stream4.forEach(System.out::println);
    }
}
// Output:
// 1
// 2
// 3
// 4
// 5
```

## Intermediate Operations:
Intermediate operations transform a stream into another stream. Some common intermediate operations include:

**1. filter():** Filters elements based on a specified condition.
**2. map():** Transforms each element in a stream to another value.
**3. Sorted():** Sorts the elements of a stream.
**4. Distinct():** Remove duplicates.
**5. Skip():** Skip first n elements.

```java
import java.util.*;
import java.util.stream.*;

public class StreamIntermediate {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(5, 10, 20, 10, 30, 40);

        numbers.stream()
               .filter(n -> n > 10)   // keep > 10
               .map(n -> n * 2)       // double them
               .distinct()            // remove duplicates
               .sorted()              // sort ascending
               .forEach(System.out::println);
    }
}
// Output:
// 40
// 60
// 80
```

## Terminal Operations:
Terminal Operations are the operations that on execution return a final result as an absolute value.

**1. ForEach():** It iterates all the elements in a stream.
**2. collect(Collectors.toList()):** It collects stream elements into a list (or other collections like set/map).
**3. Reduce():** It reduces stream elements into a single aggregated result.
**4. count():** It returns the total number of elements in a stream.
**5. anyMatch() / allMatch() / noneMatch():** They check whether elements match a given condition.
**6. findFirst() / findAny():** They return the first or any element from a stream.

```java
import java.util.*;
import java.util.stream.*;

public class StreamTerminal {
    public static void main(String[] args)
    {
        List<String> names = Arrays.asList("Amit", "Riya", "Rohan", "Amit");

        // Collect into Set (removes duplicates)
        Set<String> uniqueNames = names.stream().collect(Collectors.toSet());
        System.out.println(uniqueNames);

        // Count names starting with 'R'
        long count = names.stream().filter(n -> n.startsWith("R")).count();
        System.out.println("Names starting with R: " + count);

        // Reduce (concatenate names)
        String result = names.stream().reduce("", (a, b) -> a + b + " ");
        System.out.println(result);
    }
}
```


---
## Primitive Stream:
Java provides specialized streams for primitive data types:

**1. IntStream** -> for int values
**2. LongStream** -> for long values
**3. DoubleStream** -> for double values

```java
import java.util.stream.IntStream;

public class PrimitiveStreamDemo {
    public static void main(String[] args) {
        IntStream.range(1, 5).forEach(System.out::println);
    }
}
// Output:
// 1
// 2
// 3
// 4
```

---

## Stream vs Collection difference:
1. Collection stores data in memory and represents a data structure (e.g., List, Set, Map).
2. Stream does not store data; it processes data from a source (like a collection) in a functional, declarative way.
3. Stream cannot modifiable, cannot add or remove elements in collection.

