## final, finally and finalize:

## final:
final enforces immutability and prevents changes to variables, methods or classes.
variables - can't be changed
methods - can't be overriden
classes - can't be inherited

```java
class A {
    public static void main(String[] args)
    {
        // Non final variable
        int a = 5;

        // final variable
        final int b = 6;

        // modifying the non final variable
        a++;

        // modifying the final variable Immediately gives Compile Time error
        b++;
    }
}
// Output:
// Compile-time error saying cannot modify final variable;
```

## finally:
The finally keyword in Java is used to create a block of code that always executes after the try block, regardless of whether an exception occurs or not.

```java
public class Geeks {
    public static void main(String[] args)
    {
        try {
            System.out.println("Inside try block");
            int result
                = 10 / 0; // This will cause an exception
        }
        catch (ArithmeticException e) {
            System.out.println("Exception caught: "
                               + e.getMessage());
        }
        finally {
            System.out.println(
                "finally block always execute");
        }
    }
}
```
Output:
<img width="900" height="128" alt="image" src="https://github.com/user-attachments/assets/c250da33-b2b2-4963-bd5c-a887e79485a4" />


## finalize() Method:
The finalize() method is called by the Garbage Collector just before an object is removed from memory. It allows us to perform clean up activity.

**Note:** finalize() is deprecated in Java 9 and should not be used in modern applications. It’s better to use try-with-resources or other cleanup mechanisms instead of relying on finalize().

```java
import java.util.*;

public class Geeks {
    public static void main(String[] args)
    {
        Geeks g = new Geeks();

        System.out.println("Hashcode is: " + g.hashCode());

        // Making the object eligible for garbage collection
        g = null;

        System.gc();

        // Adding a short delay to allow GC to act
        try {
            Thread.sleep(1000);
        }
        catch (InterruptedException e) {
            e.printStackTrace();
        }

        System.out.println("End of the garbage collection");
    }

    // Defining the finalize method
    @Override protected void finalize()
    {
        System.out.println("Called the finalize() method");
    }
}
```
<img width="900" height="126" alt="image" src="https://github.com/user-attachments/assets/1d15d127-a463-409a-b20d-5aca48944bca" />
