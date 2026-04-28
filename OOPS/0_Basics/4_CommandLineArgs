## Command Line Arguments:

1. Parameter → variable in the method definition
2. Argument → actual value passed when calling the method

```java
public void add(int a, int b) {  // a, b → parameters
    System.out.println(a + b);
}

public static void main(String[] args) {
    add(10, 20); // 10, 20 → arguments
}
```

Java command-line argument is an argument, i.e., passed at the time of running the Java program. Command-line arguments passed from the console can be received by the Java program and used as input.

```java
// Java Program to Illustrate First Argument
class GFG{

    public static void main(String[] args) {

        // Printing the first argument
        System.out.println(args[0]);
    }
}
```

**Output:**
![bae406f95c9097dfed387cc9162b87d1.png](:/8cbd877cb0934ad99a41bf2ee020e8ad)

---
```java
class Hello {

    // Main driver method
    public static void main(String[] args)
    {
        // Checking if length of args array is
        // greater than 0
        if (args.length > 0) {

            // Print statements
            System.out.println("The command line"
                               + " arguments are:");

            // Iterating the args array
            // using for each loop
            for (String val : args)

                System.out.println(val);
        }
        else

            System.out.println("No command line "
                               + "arguments found.");
    }
}
```

**Output:**
![04a10627ab32847aeee8f17e1f2135c1.png](:/76daf7df27224518930914ab7eb08bf9)