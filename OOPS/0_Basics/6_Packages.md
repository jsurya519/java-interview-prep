## Packages:
A package in Java is a mechanism to group related classes, interfaces, and sub-packages into a single unit.

1. Avoiding name conflicts (two classes with the same name can exist in different packages)
2. Providing access control using public, protected, and default access
3. Reusability: packaged code can be imported and used anywhere
4. Encouraging modular programming

## Types of Packages:
<img width="800" height="237" alt="image" src="https://github.com/user-attachments/assets/0b42950b-5ac2-4e27-b024-805d32fe1a10" />


**1. Built-in Packages:**
1. java.lang
2. java.io
3. java.util
4. java.applet
5. java.awt

```java
import java.util.Random;   // built-in package

public class GFG{

    public static void main(String[] args) {

        // using Random class
        Random rand = new Random();

        // generates a number between 0–99
        int number = rand.nextInt(100);

        System.out.println("Random number: " + number);
    }
}
// Output:
// Random number: 59
```

**2. User-defined Packages**
```java
package com.myapp;

public class Helper {
    public static void show() {
        System.out.println("Hello from Helper!");
    }
}
```

To use it in another class:
```java
import com.myapp.Helper;

public class Test {
    public static void main(String[] args) {
        Helper.show();
    }
}
```

## Accessing Classes Inside a Package:

**1. Import a Single class**
```java
import java.util.Vector;
```

**2. Import all classes from a package:**
```java
import java.util.*;
```


----
**Quiz:**
What will happen if two classes in different packages have the same name and are imported in a Java file?
