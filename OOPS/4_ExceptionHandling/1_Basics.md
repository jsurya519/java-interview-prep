## Exception Handling:
Exception Handling in Java is a mechanism used to handle both compile-time (checked) and runtime (unchecked) exceptions, allowing a program to continue execution smoothly even in the presence of errors.

```java
class Geeks
{
	public static void main(String[] args)
	{
		int n=10;
		int m=0;

		int ans=n/m;
		System.out.println("Answer: "+ans);
	}
}
// Output:
// Divide by zero exception
```

## Basic try-catch Example
1. The try block contains code that might throw an exception,
2. The catch block handles the exception if it occurs.

```java
class Geeks{
    public static void main(String[] args) {

        int n = 10;
        int m = 0;

        try {
            int ans = n / m;
            System.out.println("Answer: " + ans);
        } catch (ArithmeticException e){
            System.out.println("Error: Division by 0!");
        }
    }
}
// Output
// Error: Division by 0!
```

## Finally Block:
The finally is a mandatory block, whether an exception arised or not. It is typically used for closing resources such as database connections, open files, or network connections.

Finally may not execute in cases like:
1. System.exit()
2. JVM crash
3. infinite loop before finally

```java
class FinallyExample {
    public static void main(String[] args){

        int[] numbers = { 1, 2, 3 };
        try {
            // This will throw ArrayIndexOutOfBoundsException
            System.out.println(numbers[5]);

        }
        catch (ArrayIndexOutOfBoundsException e){

            System.out.println("Exception caught: " + e);
        }
        finally{
            System.out.println("This block always executes.");
        }
        System.out.println("Program continues...");
    }
}
// Output:
// Exception caught: java.lang.ArrayIndexOutOfBoundsException: Index 5 out of bounds for length 3
// This block always executes.
// Program continues...
```

## throw and throws Keywords:

**1. throw**
Used to explicitly throw a single exception. We use throw when something goes wrong (or “shouldn’t happen”) and we want to stop normal flow

```java
class Demo {
    static void checkAge(int age) {

        if (age < 18) {
            throw new IllegalArgumentException("Age must be 18 or above");
        }
    }

    public static void main(String[] args) {

        checkAge(15);
    }
}
// Output:
// Exception in thread "main" java.lang.IllegalArgumentException: Age must be 18 or above
// 	at Demo.checkAge(Demo.java:5)
// 	at Demo.main(Demo.java:11)
```

**2. throws:**
Declares exceptions that a method might throw, informing the caller to handle them. It is mainly used with checked exceptions (explained below). If a method calls another method that throws a checked exception, and it doesn’t catch it, it must declare that exception in its throws clause

```java
import java.io.*;

class Demo {

    // Method declares that it may throw IOException
    static void readFile(String fileName) throws IOException {

        // Using try-with-resources to automatically close FileReader
        try (FileReader file = new FileReader(fileName)) {
            int data;
            while ((data = file.read()) != -1) {
                System.out.print((char) data); // Read and print file content
            }
        }
        // No need for finally block to close the resource
    }

    public static void main(String[] args) {

        try {
            readFile("test.txt"); // Attempt to read file
        } catch (IOException e) {
            System.out.println("File not found or error reading file: " + e.getMessage());
        }

        System.out.println("\nProgram continues after file operation.");
    }
}
// Output:
// File not found or error reading file: test.txt (No such file or directory)

// Program continues after file operation.
```

**Internal Working of try-catch Block:**

1. JVM executes code inside the try block.
2. If an exception occurs, remaining try code is skipped and JVM searches for a matching catch block.
3. If found, the catch block executes.
4. Control then moves to the finally block (if present).
5. If no matching catch is found, the exception is handled by JVM’s default handler. Means the program terminates abruptly and the code after it, will never execute.
6. The finally block always executes, whether an exception occurs or not.


## Java Exception Hierarchy:
In Java, all exceptions and errors are subclasses of the Throwable class. It has two main branches
1. Exception.
2. Error

![c4f26740c57756822db03396cc16ae2b.png](:/ce1b9c98a90944b986b2145ba8bc649e)

**Types of Exception:**
![728a0884508ae08caa1c67bf13758cda.png](:/32c9e3d855e44987a1d8b231bba2f948)

**1. Built-in Exception:**
Built-in Exception are pre-defined exception classes provided by Java to handle common errors during program execution. There are two types
1. Checked Exception: These exceptions are checked at compile time, forcing the programmer to handle them explicitly.
2. Unchecked Exception: These exceptions are checked at runtime and do not require explicit handling at compile time.

**2. User-Defined Exception:**
Users can also create their own exceptions.

## Nested try-catch
In Java, you can place one try-catch block inside another to handle exceptions at multiple levels.
```java
public class NestedTryExample {
    public static void main(String[] args) {
        try {
            System.out.println("Outer try block");
            try {
                int a = 10 / 0; // This causes ArithmeticException
            } catch (ArithmeticException e) {
                System.out.println("Inner catch: " + e);
            }
            String str = null;
            System.out.println(str.length()); // This causes NullPointerException
        } catch (NullPointerException e) {
            System.out.println("Outer catch: " + e);
        }
    }
}
// Output:
// Outer try block
// Inner catch: java.lang.ArithmeticException: / by zero
// Outer catch: java.lang.NullPointerException: Cannot invoke "String.length()" because "<local1>" is null
```

## Handling Multiple Exception:
We can handle multiple type of exceptions in Java by using multiple catch blocks, each catching a different type of exception.
```java
try {

    // Code that may throw an exception

} catch (ArithmeticException e) {

    // Code to handle the exception

} catch(ArrayIndexOutOfBoundsException e){

    // Code to handle the another exception

}catch(NumberFormatException e){

     // Code to handle the another exception
}
```

