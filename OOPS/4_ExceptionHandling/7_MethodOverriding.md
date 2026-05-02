**Exception Handling with Method Overriding:**

**Rules for method overriding:**

**1. Unchecked Exception:**
   (RuntimeException and its subclasses)
   No restrictions
   
**2. Checked Exception:**
Subclass are allowed to throw only anyone of the following
1. same exception
2. subclass (narrower) exception
3. no exception

```java
// Superclass without exception declaration
class SuperClass {
    void method() {
        System.out.println("SuperClass method executed");
    }
}

// Subclass declaring an unchecked exception
class SubClass extends SuperClass {
    @Override
    void method() throws ArithmeticException {
        System.out.println("SubClass method executed");
        throw new ArithmeticException("Exception in SubClass");
    }

    public static void main(String[] args) {
        SuperClass s = new SubClass();
        try {
            s.method();
        } catch (ArithmeticException e) {
            System.out.println("Caught Exception: " + e.getMessage());
        }
    }
}
// Output:
// SubClass method executed
// Caught Exception: Exception in SubClass
```


**Case 1:**

```java
import java.io.*;

class SuperClass {

  // SuperClass doesn't declare any exception
  void method() {
    System.out.println("SuperClass");
  }
}

// SuperClass inherited by the SubClass
class SubClass extends SuperClass {

  // method() declaring Checked Exception IOException
  void method() throws IOException {

    // IOException is of type Checked Exception so the compiler will give Error
    System.out.println("SubClass");
  }

  public static void main(String args[]) {
    SuperClass s = new SubClass();
    s.method();
  }
}
```

**Output:**
<img width="822" height="185" alt="image" src="https://github.com/user-attachments/assets/897fbac9-1b1b-41b9-98ab-c8c1992a5224" />


**case 2:**
```java
class SuperClass {

    // SuperClass doesn't declare any exception
    void method()
    {
        System.out.println("SuperClass");
    }
}

// SuperClass inherited by the SubClass
class SubClass extends SuperClass {

    // method() declaring Unchecked  Exception ArithmeticException
    void method() throws ArithmeticException
    {

        // ArithmeticException is of type Unchecked Exception
        System.out.println("SubClass");
    }

    public static void main(String args[])
    {
        SuperClass s = new SubClass();
        s.method();
    }
}
// Output:
// SubClass
```

**case 3:**
```java
class SuperClass {
    void method() throws RuntimeException {
        System.out.println("SuperClass");
    }
}

// Subclass declares an unrelated exception
class SubClass extends SuperClass {
    @Override
    void method() throws Exception {

        // Exception is not a child of RuntimeException
        System.out.println("SubClass");
    }

    public static void main(String[] args) {
        SuperClass o = new SubClass();
        o.method();
    }
}
```

Output:
<img width="803" height="155" alt="image" src="https://github.com/user-attachments/assets/c1e67a75-1af6-4995-a085-6d0df36b12fa" />


**case 5:**

```java
import java.io.*;

class SuperClass {

    // SuperClass declares an exception
    void method() throws RuntimeException
    {
        System.out.println("SuperClass");
    }
}

// SuperClass inherited by the SubClass
class SubClass extends SuperClass {

    // SubClass declaring a child exception of RuntimeException
    void method() throws ArithmeticException {

        System.out.println("SubClass");
    }

    public static void main(String args[])
    {
        SuperClass s = new SubClass();
        s.method();
    }
}
// Output:
// SubClass
```

**case 6:**

```java
import java.io.*;

class SuperClass {

    // SuperClass declares an exception
    void method() throws IOException
    {
        System.out.println("SuperClass");
    }
}

// SuperClass inherited by the SubClass
class SubClass extends SuperClass {

    // SubClass declaring without exception
    void method()
    {
        System.out.println("SubClass");
    }

    public static void main(String args[])
    {
        SuperClass s = new SubClass();
    try {
        s.method();
    } catch (IOException e) {
        e.printStackTrace();
    }
    }
}
// Output:
// SubClass
```
