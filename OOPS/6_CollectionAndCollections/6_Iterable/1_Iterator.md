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
![8fcd78aeb5dfdb6bbf28ea93bb58e48c.png](:/6f3ac38080d44ffb9dcef238cfb12e9d)

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

![9c386f2e614ed020f455be48321fc21a.png](:/89bea6ab3a754d2eb0e54c1704a3dbc2)

```java
citiesIterator.hasNext(); //true
citiesIterator.next(); //G-1
```

![7e493ed12221988224c87024d0ad2efd.png](:/8d2d04058db848649c5a718493735321)

```java
citiesIterator.hasNext(); // true
citiesIterator.next(); //G-2
```

![66deb6141ad7b13b8cd576ae115d3699.png](:/356ba89a07de4e17b98194fc53a23e83)

```java
citiesIterator.hasNext(); //false
```

![5be1f9b2c218c8530e3847da5bb6914b.png](:/b9f65887eec446f1a7f92fca55ae8280)

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

