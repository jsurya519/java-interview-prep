## Variable Arguments (Varargs):
Variable Arguments (Varargs) in Java allow methods to accept a variable number of parameters using a simplified syntax. Internally, varargs are handled as arrays, making them easy to process within methods.

1. It must be the last parameter in the method.
2. Only one varargs parameter is allowed per method declaration.
3. Can be combined with other parameters, but remaining arguments are grouped into an array.

```java
import java.io.*;
class Geeks {

    // Method that accepts variable number of String arguments using varargs
    public static void Names(String... n) {

        // Iterate through the array and print each name
        for (String i : n) {
            System.out.print(i + " ");
        }
        System.out.println();
    }

    public static void main(String[] args) {

        // Calling the 'Names' method with different number of arguments
        Names("geek1", "geek2");
        Names("geek1", "geek2", "geek3");
    }
}
// Output:
// geek1 geek2
// geek1 geek2 geek3
```

---
```java
class Geeks {

    // A method that takes variable number of integer arguments.
    static void fun(int... a)
    {
        System.out.println("Number of arguments: " + a.length);

        // using for each loop to display contents of a
        for (int i : a)
            System.out.print(i + " ");
        System.out.println();
    }

    // Driver code
    public static void main(String args[])
    {
        // Calling the varargs method with one parameter
        fun(100);

        // four parameters
        fun(1, 2, 3, 4);

        // no parameter
        fun();
    }
}
// Output:
// Number of arguments: 1
// 100
// Number of arguments: 4
// 1 2 3 4
// Number of arguments: 0
```

---
**Varargs with Other Arguments**
```java
class Geeks{
    // Takes string as a argument followed by varargs
    static void fun2(String s, int... a) {
        System.out.println("String: " + s);
        System.out.println("Number of arguments is: "
                           + a.length);
        // using for each loop to display contents of a
        for (int i : a)
            System.out.print(i + " ");

        System.out.println();
    }
    public static void main(String args[])
    {
        // Calling fun2() with different parameter
        fun2("GeeksforGeeks", 100, 200);
        fun2("CSPortal", 1, 2, 3, 4, 5);
        fun2("forGeeks");
    }
}
// Output:
// String: GeeksforGeeks
// Number of arguments is: 2
// 100 200
// String: CSPortal
// Number of arguments is: 5
// 1 2 3 4 5
// String: forGeeks
// Number of arguments is: 0
```

## Important Rules and Limitations:

1. Cannot specify more than one var-args in a single method.
2. Var-args should be the last parameter in the method.