## Chained Exception:
Chained Exceptions in Java allow associating one exception with another, i.e. one exception describes the cause of another exception.

**For Example:**
Consider a situation in which a method throws an ArithmeticException because of an attempt to divide by zero. But the root cause of the error was an I/O failure that caused the divisor to be zero. In such cases, chained exceptions help propagate both the primary and underlying causes of the error.

```java
// Working of chained exceptions
public class Geeks {
    public static void main(String[] args) {
        try {

            // Creating an exception
            NumberFormatException ex = new NumberFormatException("Primary Exception");

            // Setting the cause of the exception
            ex.initCause(new NullPointerException("Root cause of the exception"));

            // Throwing the exception with a cause
            throw ex;
        }
        catch (NumberFormatException ex) {

            // Displaying the primary exception
            System.out.println("Caught Exception: " + ex);

            // Displaying the root cause of the exception
            System.out.println("Cause of Exception: " + ex.getCause());
        }
    }
}
// Output:
// Caught Exception: java.lang.NumberFormatException: Primary Exception
// Cause of Exception: java.lang.NullPointerException: Root cause of the exception
```

## Methods of Throwable Supporting Chained Exceptions
1. **getCause():** This method returns actual cause of an exception.
2. **initCause(Throwable cause):** This method sets the cause for the calling exception.

## Constructors for Chained Exceptions:
1. **Throwable(Throwable cause):** Where cause is the exception that causes the current exception.
2. **Throwable(String msg, Throwable cause):** Where msg is the exception message and cause is the exception that causes the current exception.

```java
// Use a custom message with chained exception
public class Geeks {
    public static void main(String[] args) {
        try {

            // Code that might throw an exception
            int[] n = new int[5];
            int divisor = 0;

            for (int i = 0; i < n.length; i++) {
                int res = n[i] / divisor;
                System.out.println(res);
            }
        }
        catch (ArithmeticException e) {

            // Creating a new exception with
            // the original as the cause
            throw new RuntimeException
              ("Error: Division by zero occurred", e);
        }
    }
}
```

**Output:**
<img width="1013" height="290" alt="image" src="https://github.com/user-attachments/assets/ea9ce88f-4e24-4192-bc30-4940b801545a" />


**Note:**
Normal Exception - Throwable(String msg);
```java
throw new RuntimeException("Runtime Exception");
```

