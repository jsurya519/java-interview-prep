## Set Interface:
1. The set interface does not allow duplicate elements.
2. It can contain at most one null value except TreeSet implementation which does not allow null.
3. The set interface provides efficient search, insertion, and deletion operations.

**Set Interface Example:**
```java
import java.util.HashSet;
import java.util.Set;

public class Geeks {

  	public static void main(String args[])
    {
        // Create a Set using HashSet
        Set<String> s = new HashSet<>();

        // Displaying the Set
        System.out.println("Set Elements: " + s);
    }
}
```

In the above example, HashSet will appear as an empty set, as no elements were added. **The order of elements in HashSet is not guaranteed, so the elements will be displayed in a random order if any are added.**

## Common Operations in Set:

**1. Adding Elements:**
To add elements to a Set in Java, use the add() method.

**2. Accessing the Elements:**
After adding the elements, if we wish to access the elements, we can use inbuilt methods like contains().

**3. Removing Elements:**
The values can be removed from the Set using the remove() method.

**4. Iterating elements:**
There are various ways to iterate through the Set. The most famous one is to use the enhanced for loop.
