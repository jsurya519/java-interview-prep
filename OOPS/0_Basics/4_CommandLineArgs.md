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
<img width="851" height="268" alt="image" src="https://github.com/user-attachments/assets/03657e05-a621-4ea2-930c-94804cf0b7ae" />


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
<img width="649" height="167" alt="image" src="https://github.com/user-attachments/assets/f85332f4-cde7-4730-bab6-982b8ae5dbf9" />
