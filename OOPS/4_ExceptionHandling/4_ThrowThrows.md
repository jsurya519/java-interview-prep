## throw keyword:
The throw keyword in Java is used to explicitly throw an exception from a method or any block of code. We can throw either checked or unchecked exception.
```java
throw instance
```
 Instance must be of type Throwable or a subclass of Throwable.

When a throw statement is executed, the program flow immediately stops and the nearest try block is checked for a matching catch block.
1. If a matching catch block is found, control is transferred to that block.
2. If no match is found, the default exception handler terminates the program.
```java
class Geeks {
    static void fun()
    {
        try {
            throw new NullPointerException("demo");
        }
        catch (NullPointerException e) {
            System.out.println("Caught inside fun().");
            throw e;     // rethrowing the exception
        }
    }

    public static void main(String args[])
    {
        try {
            fun();
        }
        catch (NullPointerException e) {
            System.out.println("Caught in main.");
        }
    }
}
// Output:
// Caught inside fun().
// Caught in main.
```

```java
class Geeks {
    public static void main(String[] args){
        int numerator = 1;
        int denominator = 0;

        if (denominator == 0) {
            // Manually throw an ArithmeticException
            throw new ArithmeticException("Cannot divide by zero");
        } else {
            System.out.println(numerator / denominator);
        }
    }
}
// Output:
// Hangup (SIGHUP)
// Exception in thread "main" java.lang.ArithmeticException: Cannot divide by zero at Geeks.main(Geeks.java:9)
```

## throws Keyword:
throws is a keyword in Java that is used in the signature of a method to indicate that this method might throw one of the listed type exceptions. The caller to these methods has to handle the exception using a try-catch block.
```java
type method_name(parameters) throws exception_list
```
where, exception_list is a comma separated list of all the exceptions which a method might throw.

If a method can throw a checked exception, the compiler requires it to be either handled using a try-catch block or declared using the throws keyword; otherwise, a compile-time error occurs.
1. The exception can be handled using a try-catch block.
2. The throws keyword can be used to declare the exception and delegate the handling responsibility to the caller (method or JVM).

```java
class Geeks {

    static void fun() throws IllegalAccessException
    {
        System.out.println("Inside fun(). ");
        throw new IllegalAccessException("demo");
    }

    public static void main(String args[])
    {
        try {
            fun();
        }
        catch (IllegalAccessException e) {
            System.out.println("Caught in main.");
        }
    }
}
// Output:
// Inside fun().
// Caught in main.
```