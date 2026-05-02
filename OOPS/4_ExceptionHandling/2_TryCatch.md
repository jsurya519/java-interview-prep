## Try Catch Block:
1. The try block contains code that may generate an exception.
2. The catch block handles the exception if it occurs.

```java
import java.io.*;

class Geeks {
    public static void main(String[] args) {
        try {

            // This will throw an ArithmeticException
            int res = 10 / 0;
        }
        // Here we are Handling the exception
        catch (ArithmeticException e) {
            System.out.println("Exception caught: " + e);
        }

        // This line will executes weather an exception
        // occurs or not
        System.out.println("I will always execute");
    }
}
// Output:
// Exception caught: java.lang.ArithmeticException: / by zero
// I will always execute
```

## Internal working of try-catch Block:
1. Java Virtual Machine starts executing the code inside the try block.
2. If an exception occurs, the remaining code in the try block is skipped, and the JVM starts looking for the matching catch block.
3. If a matching catch block is found, the code in that block is executed.
4. After the catch block, control moves to the finally block (if present).
5. If no matching catch block is found the exception is passed to the JVM default exception handler.
6. The finally block executes in most cases, even if an exception occurs, but it may not execute if the JVM exits abruptly (e.g., via System.exit()), crashes, or there’s an infinite loop before finally.

```java
import java.util.*;

public class Geeks {
    public static void main(String[] args) {
        try {
            // Outer try block
            System.out.println("Outer try block started");

            // Inner try block 1: handles ArithmeticException
            try {
                int n = 10;
                int res = n / 0;  // This will throw ArithmeticException
            } catch (ArithmeticException e) {
                System.out.println("Caught ArithmeticException in inner try-catch: " + e);
            }

            // Inner try block 2: handles NullPointerException
            try {
                String s = null;
                System.out.println(s.length());  // This will throw NullPointerException
            } catch (NullPointerException e) {
                System.out.println("Caught NullPointerException in inner try-catch: " + e);
            }

        } catch (Exception e) {
            // Outer catch block (optional general handler)
            System.out.println("Caught exception in outer try-catch: " + e);
        } finally {
            // Finally block executes in most cases
            System.out.println("Finally block executed");
        }

        System.out.println("Program continues after nested try-catch");
    }
}

```

```java
import java.util.*;

public class Geeks {
    public static void main(String[] args) {
        try {
            System.out.println("Outer try block started");

            // Inner try block 1
            try {
                int n = 10;
                int res = n / 0;
            } catch (ArithmeticException e) {
                System.out.println("Caught ArithmeticException in inner try-catch: " + e);
            }

            // Inner try block 2
            try {
                String s = null;
                System.out.println(s.length());
            } catch (NullPointerException e) {
                System.out.println("Caught NullPointerException in inner try-catch: " + e);
            }

        } catch (Exception e) {
            System.out.println("Caught exception in outer try-catch: " + e);
        } finally {
            System.out.println("Finally block executed");
        }

        System.out.println("Program continues after nested try-catch");
    }
}
```
