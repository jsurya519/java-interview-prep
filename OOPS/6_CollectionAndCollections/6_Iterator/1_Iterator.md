## Iterator:
 1. It is used to traverse or iterate through elements of a collection one by one.
 2. It is used to traverse elements in the forward direction only.
 3. Removes elements safely during traversal using remove().
 4. Iterator is a universal cursor that applies to most collection types such as List, Set, and Queue.

```java
import java.util.ArrayList;
import java.util.Iterator;

public class Geeks {
    public static void main(String[] args) {

        // Create an ArrayList and add some elements
        ArrayList<String> al = new ArrayList<>();
        al.add("A");
        al.add("B");
        al.add("C");

        // Obtain an iterator for the ArrayList
        Iterator<String> it = al.iterator();

        // Iterate through the elements and print each one
        while (it.hasNext()) {

            // Get the next element
            String n = it.next();
            System.out.println(n);
        }
    }
}
// Output:
// A
// B
// C
```

## Hierarchy of Iterator:
<img width="465" height="235" alt="image" src="https://github.com/user-attachments/assets/6274c5f9-2802-4fdb-952b-9ea35b2eda41" />


All concrete classes of List, Queue and Set interfaces has iterator() implementation.

## Methods of Iterator Interface:
**1. hasNext():** Returns true if the iteration has more elements.
**2. next():** Returns the next element in the iteration. It throws NoSuchElementException if no more element is present.
**3. remove():** Removes the last element returned by next(). This method can be called only once per call to next().Calling it more than once without calling next() will throw IllegalStateException.
```java
Iterator<Integer> it = list.iterator();
it.next();
it.remove();  // Works fine
it.remove();  // Throws IllegalStateException
```

## Internal Working:

```java
Iterator<String> citiesIterator = cities.iterator();
```

<img width="542" height="240" alt="image" src="https://github.com/user-attachments/assets/2e280c12-0007-4f51-a4ad-3e2afb12666a" />


```java
citiesIterator.hasNext(); //true
citiesIterator.next(); //G-1
```

<img width="562" height="254" alt="image" src="https://github.com/user-attachments/assets/52f4eac1-4f20-42a2-970f-ba20ad510f67" />


```java
citiesIterator.hasNext(); // true
citiesIterator.next(); //G-2
```

<img width="559" height="246" alt="image" src="https://github.com/user-attachments/assets/73105c7f-658f-4987-8ac6-6de66f5f0bc5" />


```java
citiesIterator.hasNext(); //false
```

<img width="658" height="330" alt="image" src="https://github.com/user-attachments/assets/77cdce3c-9834-4c54-8cf4-d8d13ac9ab17" />


```java
import java.util.ArrayList;
import java.util.Iterator;

public class Geeks {

    public static void main(String[] args) {

        // Creating an ArrayList of Integer type
        ArrayList<Integer> al = new ArrayList<>();

        // Adding elements to the ArrayList
        for (int i = 0; i < 10; i++) {
            al.add(i);
        }

        // Printing the original list
        System.out.println("Original List: " + al);

        // Creating an Iterator for the ArrayList
        Iterator<Integer> itr = al.iterator();

        // Iterating through the list and removing odd elements
        while (itr.hasNext()) {

            // Getting the next element
            int i = itr.next();

            System.out.print(i + " ");

            // Removing odd elements
            if (i % 2 != 0) {
                itr.remove();
            }
        }

        System.out.println();

        // Printing the modified list after removal of odd elements
        System.out.println("Modified List: " + al);
    }
}
// Output:
// Original List: [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
// 0 1 2 3 4 5 6 7 8 9
// Modified List: [0, 2, 4, 6, 8]
```

