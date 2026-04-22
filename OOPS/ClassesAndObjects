## Class:
A class is a blueprint used to create objects. It groups fields and methods in a single unit. It can contain fields, methods, constructors, nested classes and interfaces. Memory for instance members is allocated when an object is created, while static members are allocated when class is loaded into memory.

## Objects:
An object is an instance of a class. Creating an object is known as instantiation. All instances of class share structure and behaviour while storing different states of values.

Example:
```java
class Product
{
    String name;
    float price;

    public Product(String name, float price)
    {
        this.name=name;
        this.price=price;
    }

    public String getName()
    {
        return name;
    }

    public float getPrice()
    {
        return price;
    }
}

public class Main
{
    public static void main(String[] args)
    {
        Product p1 = new Product("Visual studio", 0.0f);
        Product p2 = new Product("Intellij", 499.0f);

        System.out.println(p1.getName() + " - " + p1.getPrice());
        System.out.println(p2.getName() + " - " + p2.getPrice());
    }
}
```


## Ways to create Object:
1. Using new Keyword
2. Using clone() method
3. Using Deserialization
4. Using Reflection

## 1. Using new keyword
new Keyword is most direct way to create an object.
```java
// creating object of class Test
Test t = new Test();
```

## 2. Using clone() method
clone() creates a copy of an existing object. The class must implement Cloneable.

```java
class Geeks implements Cloneable {
    String name = "GeeksForGeeks";

    @Override
    protected Object clone() throws CloneNotSupportedException {
        return super.clone();
    }

    public static void main(String[] args) throws CloneNotSupportedException {
            Geeks g1 = new Geeks();
            Geeks g2 = (Geeks) g1.clone();
            System.out.println(g2.name);
    }
}
```

## 3. Using Deserialization
De-serialization is a technique of reading an object from the saved state in a file. Object is recreated from a stored byte stream.

## 4. Using Reflection
Used for dynamic class loading as seen in frameworks like Spring.

---

## Anonymous Object

An anonymous object is an object that is created without giving it a name. Anonymous objects are often used to create objects on the fly and pass them as arguments to methods.

```java
import java.io.*;

class Person {
    String name;
    int age;

    Person(String name, int age)
    {
        this.name = name;
        this.age = age;
    }

    void display()
    {
        System.out.println("Name: " + name
                           + ", Age: " + age);
    }
}

public class Main {
    public static void main(String[] args)
    {
        // Create an anonymous object
        // and call its display method
        new Person("John", 30).display();
    }
}
```


Anonymous objects are often used in combination with inner classes, anonymous classes, and lambda expressions. Example for how to use an anonymous object with an inner class:

```java
import java.io.*;

class OuterClass {
    class InnerClass {
        void display()
        {
            System.out.println("Inside InnerClass");
        }
    }

    public static void main(String[] args)
    {
        // Create an anonymous object of the InnerClass and
        // call its display method
        new OuterClass().new InnerClass().display();
    }
}
```