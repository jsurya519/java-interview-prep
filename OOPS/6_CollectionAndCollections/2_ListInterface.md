## 1. List Interface:
1. List interface extends Collection interface
2. Maintains insertion order
3. Allows duplicate elements
4. Supports null elements (implementation dependent)
5. Supports bidirectional traversal using ListIterator

## Commonly used methods in List:

**1. Adding Elements:**

**add(Object o):** This method is used to add an element at the end of the List.
**add(int index, Object o):** This method is used to add an element at a specific index in the List

**2. Updating Elements:**
**set(Object o):** Updates target index with new value;

**3. Searching Elements:**

**indexOf(Object o):** It returns the index of the first occurrence of the specified element in the list or -1 if the element is not found
**lastIndexOf(Object o):** It returns the index of the last occurrence of the specified element in the list or -1 if the element is not found

**4. Removing Elements:**
**remove(Object o):** Removes the first occurrence of the specified object from the list.
**remove(int index):** Removes the element at the specified index and shifts subsequent elements left.

**5. Accessing Elements**
**get(int index):** This method returns the element at the specified index in the list.

**6. Checking if an element is present or not**
**contains(Object o):** This method takes a single parameter, the object to be checked if it is present in the list.

---
## Iterating over List Interface in Java
1. Basic for loop with get(index)
2. Enhanced for-each loop

```java
import java.util.*;

public class Geeks {

    public static void main(String args[])
    {
        // Creating an empty Arraylist of string type
        List<String> al = new ArrayList<>();

        // Adding elements to above object of ArrayList
        al.add("Geeks");
        al.add("Geeks");

        // Adding element at specified position inside list object
        al.add(1, "For");

        // Using  for loop for iteration
        for (int i = 0; i < al.size(); i++) {

            // Using get() method to access particular element
            System.out.print(al.get(i) + " ");
        }

        // New line for better readability
        System.out.println();

        // Using for-each loop for iteration
        for (String str : al)

            System.out.print(str + " ");
    }
}
// Ouput:
// Geeks For Geeks
// Geeks For Geeks
```

