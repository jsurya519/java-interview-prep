## Generics:
Generics in Java refer to **parameterized types** that allow writing code which works with multiple data types using a single class, interface, or method.

Consider below example,

void fun(int x);

Here

void -> represents return data type

x -> represent parameter

int -> parameter type, but it is actual type. i.e int

Another example,

void fun(T x);

Here

x -> represents parameter

T -> **"type-parameter"** used as parameter type

The actual type is decided when the generic class/interface/method is used.

```java
class Demo<T> {
    void fun(T x) {
        System.out.println(x);
    }
}
```

**Usage:**
```java
Demo<String> d1 = new Demo<>();
Demo<Integer> d2 = new Demo<>();
```

---
## Types of Generics:
**1. Generic class**
A generic class is a class that can operate on objects of different types using a type parameter.

```java
// We use < > to specify Parameter type
class Test<T> {

    T obj;
    Test(T obj) {
        this.obj = obj;
    }
    public T getObject() { return this.obj; }
}

class Geeks {
    public static void main(String[] args)
    {
        // instance of Integer type
        Test<Integer> iObj = new Test<Integer>(15);
        System.out.println(iObj.getObject());

        // instance of String type
        Test<String> sObj
            = new Test<String>("GeeksForGeeks");
        System.out.println(sObj.getObject());
    }
}
// Output:
// 15
// GeeksForGeeks
```

**Note:**
1. In Parameter type, we can not use primitives like "int", "char" or "double". We should only use reference types.
2. In a generic class, the type parameter T behaves like a normal data type within the class. Once a specific type is provided while creating an object, the compiler replaces T with that type.
---
We can also pass multiple Type parameters in Generic classes.
```java
class Test<T, U>
{
    T obj1;  // An object of type T
    U obj2;  // An object of type U

    Test(T obj1, U obj2)
    {
        this.obj1 = obj1;
        this.obj2 = obj2;
    }

    public void print()
    {
        System.out.println(obj1);
        System.out.println(obj2);
    }
}

class Geeks
{
    public static void main (String[] args)
    {
        Test <String, Integer> obj =
            new Test<String, Integer>("GfG", 15);

        obj.print();
    }
}
// Output:
// GfG
// 15
```

**2. Generic Method:**
We can also write generic methods that can be called with different types of arguments based on the type of arguments passed to the generic method. The compiler handles each method.
```java
class Geeks {

    // A Generic method example
    static <T> void genericDisplay(T element)
    {
        System.out.println(element.getClass().getName()
                           + " = " + element);
    }

    public static void main(String[] args)
    {
        // Calling generic method with Integer argument
        genericDisplay(11);

        // Calling generic method with String argument
        genericDisplay("GeeksForGeeks");

        // Calling generic method with double argument
        genericDisplay(1.0);
    }
}
// Output:
// java.lang.Integer = 11
// java.lang.String = GeeksForGeeks
// java.lang.Double = 1.0
```

## Points to remember on generics:
**1. Generics works only with reference types**
```java
Test<int> obj = new Test<int>(20); //compile time error
```
The above line results in a compile-time error that can be resolved using type wrappers to encapsulate a primitive type.  But primitive type arrays can be passed to the type parameter because arrays are reference types.
```java
ArrayList<int[]> a = new ArrayList<int[]>(); //Allowed, because array is a reference
```

**2. Diamond Operator (<>) in Java**
Instead of writing the type parameter again on the right-hand side, the compiler can automatically infer the type from the left-hand side.
```java
//Both are correct
ArrayList<int[]> a = new ArrayList<int[]>();
ArrayList<int[]> a = new ArrayList<>(); // Using diamond operator (<>), Java infers the type automatically
```

**3. Generic types differ based on their type arguments**
During compilation, java removes generic type and replaces type-paramter with their bounds or object.
```java
class Test<T> {
    // An object of type T is declared
    T obj;
    Test(T obj) { this.obj = obj; } // constructor
    public T getObject() { return this.obj; }
}

class Geeks {
    public static void main(String[] args)
    {
        // instance of Integer type
        Test<Integer> iObj = new Test<Integer>(15);
        System.out.println(iObj.getObject());

        // instance of String type
        Test<String> sObj
            = new Test<String>("GeeksForGeeks");
        System.out.println(sObj.getObject());
        iObj = sObj; // This results an error
    }
}
// Output:
// error:
//  incompatible types:
//  Test cannot be converted to Test
```
**Explanation:** At compile time, Test<Integer> and Test<String> are treated as different parameterized types.
Java generics enforce type safety during compilation, so assigning one to another results in a compile-time error.

**4. Static Variables in Generic Classes**
Static members are shared across all type parameters of a generic class.

```java
class Test<T> {
    static int count = 0;

    Test() {
        count++;
    }
}

public class Geeks {
    public static void main(String[] args) {

        Test<Integer> obj1 = new Test<>();
        Test<String> obj2 = new Test<>();
        Test<Double> obj3 = new Test<>();

        System.out.println(Test.count);
    }
}
// Output:
// 3
```

---
## Type Parameter Naming Conventions:
The common type parameters are as follows:

T: Type

E: Element

K: Key

N: Number

V: Value

---
## Without Generics:
```java
import java.util.*;

class Geeks
{
    public static void main(String[] args)
    {
        // Creatinga an ArrayList without any type specified
        ArrayList al = new ArrayList();

        al.add("Sweta");
        al.add("Gudly");
        al.add(10); // Compiler allows this

        String s1 = (String)al.get(0);
        String s2 = (String)al.get(1);

        // Causes Runtime Exception
        String s3 = (String)al.get(2);
    }
}
// Output:
// Exception in thread "main" java.lang.ClassCastException:
//    java.lang.Integer cannot be cast to java.lang.String
//     at Test.main(Test.java:19)
```

Without generics, collections store elements as Object. This allows inserting different data types (e.g., adding an Integer in a list of String). The issue is detected only at runtime, leading to a ClassCastException.

## With Generics:
```java
import java.util.*;

class Geeks
{
    public static void main(String[] args)
    {
        // Creating a an ArrayList with String specified
        ArrayList <String> al = new ArrayList<String> ();

        al.add("Sweta");
        al.add("Gudly");

        // Now Compiler doesn't allow this
        al.add(10);

        String s1 = (String)al.get(0);
        String s2 = (String)al.get(1);
        String s3 = (String)al.get(2);
    }
}
```

**Individual Type Casting is not needed:**
 Generics remove the need for manual type casting during retrieval, making code cleaner and safer.
 ```java
import java.util.*;

class Geeks {
    public static void main(String[] args)
    {
        // Creating a an ArrayList with String specified
        ArrayList<String> al = new ArrayList<String>();

        al.add("Sweta");
        al.add("Gudly");

        // Typecasting is not needed
        String s1 = al.get(0);
        String s2 = al.get(1);
    }
}
 ```

Without generics, we need manually typecast from object to respective class type.
