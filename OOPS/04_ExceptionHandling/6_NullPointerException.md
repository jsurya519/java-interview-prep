## Null Pointer Exception:
A NullPointerException in Java is a RuntimeException. It occurs when a program attempts to use an object reference that has the null value. In Java, "null" is a special value that can be assigned to object references to indicate the absence of a value.

## How to Avoid NullPointerException:
To avoid the NullPointerException, we must ensure that all the objects are initialized properly, before we use them. When we declare a reference variable, we must verify that object is not null, before we request a method or a field from the objects.

**1. Using String Literals in equals()**
```java
import java.io.*;

class Geeks {
    public static void main (String[] args) {

        // Initializing String variable with null value
        String s = null;

        // Checking if s.equals null
        try
        {
            // This line of code throws NullPointerException because s is null
            if (s.equals("gfg"))
                System.out.print("Same");
            else
                System.out.print("Not Same");
        }
        catch(NullPointerException e)
        {
            System.out.print("NullPointerException Caught");
        }
    }
}
// Output:
// NullPointerException Caught
```

We can avoid NullPointerException by calling equals on literal rather than object.
```java
import java.io.*;

class Geeks {
    public static void main (String[] args) {

        // Initializing String variable with null value
        String s = null;

        // Checking if s is null using try catch
        try
        {
            if ("gfg".equals(s))
                System.out.print("Same");
            else
                System.out.print("Not Same");
        }
        catch(NullPointerException e)
        {
            System.out.print("Caught NullPointerException");
        }
    }
}
// Output:
// Not Same
```

**2. Checking Method Arguments**

```java
import java.io.*;

class Geeks {
    public static void main(String[] args) {

        // String s set an empty string and calling getLength()
        String s = "";

        try {
            System.out.println(getLength(s));
        }
        catch (IllegalArgumentException e) {
            System.out.println(
                "IllegalArgumentException caught");
        }

        // String s set to a value and calling getLength()
        s = "GeeksforGeeks";

        try {
            System.out.println(getLength(s));
        }
        catch (IllegalArgumentException e) {
            System.out.println(
                "IllegalArgumentException caught");
        }

        // Setting s as null and calling getLength()
        s = null;

        try {
            System.out.println(getLength(s));
        }
        catch (IllegalArgumentException e) {
            System.out.println(
                "IllegalArgumentException caught");
        }
    }

    public static int getLength(String s)
    {
        if (s == null)
            throw new IllegalArgumentException(
                "The argument cannot be null");

        return s.length();
    }
}
// Output:
// 0
// 13
// IllegalArgumentException caught
```

**3. Use Ternary Operator**

```java
import java.io.*;

class Geeks {
    public static void main(String[] args)
    {
        String s = null;
        String m = (s == null) ? "" : s.substring(0, 5);

        System.out.println(m);

        s = "Geeksforgeeks";
        m = (s == null) ? "" : s.substring(0, 5);
        System.out.println(m);
    }
}
// Output:
// Geeks
```

