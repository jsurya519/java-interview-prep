## ForEachLoop:
The for-each loop in Java (introduced in Java 5) provides a simple, readable way to iterate over arrays and collections without using indexes.

```java
for (datatype variable : arrayOrCollection) {
// statements using variable
}
```

```java
class Geeks {

    public static void main(String[] args) {

        int[] arr = {1, 2, 3, 4, 5};

        // Using for-each loop
        for (int e : arr) {
            System.out.print(e + " ");
        }
    }
}
// Output:
// 1 2 3 4 5
```

---
**Example 1:**
```java
class Geeks {

    public static void main(String[] args) {

        int[] marks = {125, 132, 95, 116, 110};
        int max = findMax(marks);

        System.out.println(max);
    }
    static int findMax(int[] arr) {
        int maximum = arr[0];

        for (int value : arr) {
            if (value > maximum) {
                maximum = value;
            }
        }
        return maximum;
    }
}
// Output:
// 132
```

**Example 2:**
```java
import java.util.*;

class Geeks {

    public static void main(String[] args) {

        List<Integer> list = new ArrayList<>();
        list.add(3);
        list.add(5);
        list.add(7);
        list.add(9);

        int max = Integer.MIN_VALUE;

        for (int num : list) {
            if (num > max) {
                max = num;
            }
        }

        System.out.println("List of Integers: " + list);
        System.out.println("Maximum element: " + max);
    }
}
// Output:
// List of Integers: [3, 5, 7, 9]
// Maximum element: 9
```

## Limitations of the For-each Loop

**1. Cannot Modify Primitive Array Elements**
```java
for (int num : marks) {
num = num * 2;  // Does not modify array
}
```

The loop variable holds a copy of the value, not the actual array element.

**2. No Access to Index**
```java
for (int num : numbers) {
if (num == target) {
// do not know the index of 'num' here
return ???;     // Index is unavailable in for-each loop
}
}
```

The for-each loop does not provide access to the index of the current element. If we need the index for any reason (e.g., in a search operation), a traditional loop would be more appropriate.

**3. Single-direction Iteration Only**
```java
// Traditional reverse iteration
for (int i = numbers.length - 1; i >= 0; i--) {
System.out.println(numbers[i]); // Reverse iteration not possible with for-each
}
```

The for-each loop allows only forward iteration. Reverse iteration requires a traditional for loop with an index.