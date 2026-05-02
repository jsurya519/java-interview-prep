## User-Defined Custom Exception:
A user-defined custom exception is an exception class created by the programmer to represent application-specific or business-specific error scenarios.

**Examples of User-defined Exception:**
1. Invalid bank transaction
2. Insufficient balance
3. Age not eligible for registration
4. Invalid login attempt

Types of custom exceptions:
![82c291f26e697cda9247090af6d90c3c.png](:/bcbdfe421fe143709ed5d987d35cf9c3)

1. **Checked Exceptions:** It extends the Exception class. and it must be declared in the throws clause of the method signature.
2. **Unchecked Exceptions:** It extend RuntimeException and are not checked by the compiler, so they don’t need to be declared or handled.

**Example for Checked Exception:**

```java
// Custom Checked Exception
class InvalidAgeException extends Exception {
    public InvalidAgeException(String m) {
        super(m);
    }
}

// Using the Custom Exception
public class Geeks {
    public static void validate(int age)
      throws InvalidAgeException {
        if (age < 18) {
            throw new InvalidAgeException("Age must be 18 or above.");
        }
        System.out.println("Valid age: " + age);
    }

    public static void main(String[] args) {
        try {
            validate(12);
        } catch (InvalidAgeException e) {
            System.out.println("Caught Exception: " + e.getMessage());
        }
    }
}
// Output:
// Caught Exception: Age must be 18 or above.
```

**Example for unchecked Exception:**
```java
// Custom Unchecked Exception
class DivideByZeroException extends RuntimeException {
    public DivideByZeroException(String m) {
        super(m);
    }
}

// Using the Custom Exception
public class Geeks {
    public static void divide(int a, int b) {
        if (b == 0) {
            throw new DivideByZeroException("Division by zero is not allowed.");
        }
        System.out.println("Result: " + (a / b));
    }

    public static void main(String[] args) {
        try {
            divide(10, 0);
        } catch (DivideByZeroException e) {
            System.out.println("Caught Exception: " + e.getMessage());
        }
    }
}
// Output:
// Caught Exception: Division by zero is not allowed.
```

