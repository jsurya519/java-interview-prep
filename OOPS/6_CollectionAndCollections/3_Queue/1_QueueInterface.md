## Queue Interface:
1. Elements follow FIFO (First-In-First-Out) in LinkedList and priority order in PriorityQueue
2. Elements cannot be accessed directly using an index
3. Allows storing duplicate elements

## Common Operations in Queue Interface:

**1. Adding Elements**
To add an element in a queue, we can use the add() method. The insertion order is not retained in the PriorityQueue. The elements are stored based on the priority order which is ascending by default.

```java
import java.util.*;

public class Geeks {

    public static void main(String args[])
    {
        Queue<String> pq = new PriorityQueue<>();

        pq.add("Geeks");
        pq.add("For");
        pq.add("Geeks");

        System.out.println(pq);
    }
}
// Output:
// [For, Geeks, Geeks]
```

**2. Removing Elements**
To remove an element from a queue, we can use the remove() method. If there are multiple objects, then the first occurrence of the object is removed. The poll() method is also used to remove the head and return it.

```java
import java.util.*;

public class Geeks {

    public static void main(String args[])
    {
        Queue<String> pq = new PriorityQueue<>();

        pq.add("Geeks");
        pq.add("For");
        pq.add("Geeks");

        System.out.println("Initial Queue: " + pq);

        pq.remove("Geeks");

        System.out.println("After Remove: " + pq);

        System.out.println("Poll Method: " + pq.poll());

        System.out.println("Final Queue: " + pq);
    }
}
// Output:
// Initial Queue: [For, Geeks, Geeks]
// After Remove: [For, Geeks]
// Poll Method: For
// Final Queue: [Geeks]
```

**3. Accessing Elements:**
We can access the head element without removing it using peek() or element().

```java
import java.util.PriorityQueue;
import java.util.Queue;

public class Geeks{

    public static void main(String[] args) {
        // Create a PriorityQueue of Strings
        Queue<String> pq = new PriorityQueue<>();

        // Adding elements to the queue
        pq.add("Geeks");
        pq.add("For");
        pq.add("Geeks");

        // Access the head element without removing
        System.out.println("Head using peek(): " + pq.peek());
        System.out.println("Head using element(): " + pq.element());

        // Display the queue to show elements are not removed
        System.out.println("Queue after accessing head: " + pq);
    }
}
// Output:
// Head using peek(): For
// Head using element(): For
// Queue after accessing head: [For, Geeks, Geeks]
```

**4. Iterating the Queue**
There are multiple ways to iterate through the Queue. The most famous way is converting the queue to the array and traversing using the for loop. The queue has also an inbuilt iterator which can be used to iterate through the queue.

```java
import java.util.*;

public class Geeks {

    public static void main(String args[])
    {
        Queue<String> pq = new PriorityQueue<>();

        pq.add("Geeks");
        pq.add("For");
        pq.add("Geeks");
         // use Type safe Iterator
        Iterator<String> iterator = pq.iterator();

        while (iterator.hasNext()) {
            System.out.print(iterator.next() + " ");
        }
    }
}
// Output:
// For Geeks Geeks
```

