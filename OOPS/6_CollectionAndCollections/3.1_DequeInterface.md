## Deque Interface:
 It stands for Double-Ended Queue and represents a linear collection that allows insertion, removal, and retrieval of elements from both ends.
1. Allows insertion and deletion from both front and rear
2. Can be used as both stack (LIFO) and queue (FIFO)
3. Common implementations are ArrayDeque and LinkedList

## Common Operations in Deque Interface:

**1. Adding Elements**
To add elements in a Deque, you can use add() , addFirst(), or addLast(), allowing insertion at either end, unlike a standard Queue which only adds at the tail.

```java
import java.util.*;
public class ArrayDequeDemo {
    public static void main(String[] args)
    {
        // Initializing an deque
        Deque<String> dq = new ArrayDeque<String>();

        // add() method to insert
        dq.add("For");
        dq.addFirst("Geeks");
        dq.addLast("Geeks");

        System.out.println(dq);
    }
}
// Output:
// [Geeks, For, Geeks]
```

**2. Removing Elements**
Elements can be removed from both ends using removeFirst(), removeLast(), pop(), or poll() variants; pop() throws an exception if empty, while poll() is safe.

```java
import java.util.*;

public class ArrayDequeDemo {

    public static void main(String[] args)
    {
        // Initializing an deque
        Deque<String> dq = new ArrayDeque<String>();

        // add() method to insert
        dq.add("For");
        dq.addFirst("Geeks");
        dq.addLast("Geeks");

        System.out.println(dq);

        System.out.println(dq.pop());

        System.out.println(dq.poll());

        System.out.println(dq.pollFirst());

        System.out.println(dq.pollLast());
    }
}
// Output:
// [Geeks, For, Geeks]
// Geeks
// For
// Geeks
// null
```

**3. Iterating through the Deque**
Since a Deque can be iterated from both directions, the iterator method of the Deque interface provides us two ways to iterate. One from the first and the other from the back.

```java
import java.util.*;

public class ArrayDequeDemo {
    public static void main(String[] args)
    {
        // Initialize Deque
        Deque<String> dq = new ArrayDeque<>();
        dq.add("For");
        dq.addFirst("Geeks");
        dq.addLast("Geeks");
        dq.add("is so good");

        // Type-safe forward iteration
        for (Iterator<String> itr = dq.iterator();
             itr.hasNext();) {
            System.out.print(itr.next() + " ");
        }
        System.out.println();

        // Type-safe reverse iteration
        for (Iterator<String> itr = dq.descendingIterator();
             itr.hasNext();) {
            System.out.print(itr.next() + " ");
        }
    }
}
// Output:
// Geeks For Geeks is so good
// is so good Geeks For Geeks
```