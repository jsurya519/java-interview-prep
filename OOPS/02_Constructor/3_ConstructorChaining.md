## Constructor chaining
Constructor chaining is the process of calling one constructor from another constructor. It is used to avoid duplicate codes while having multiple constructor (by means of constructor overloading) and make code more readable.

## Constructor chaining types:
1. Within same class
2. From base class

**1. Within same class**

It can be done using this() keyword for constructors in the same class

```java
// Java program to illustrate Constructor Chaining
// within same class Using this() keyword
class Temp
{
    // default constructor 1
    // default constructor will call another constructor
    // using this keyword from same class
    Temp()
    {
        // calls constructor 2
        this(5);
        System.out.println("The Default constructor");
    }

    // parameterized constructor 2
    Temp(int x)
    {
        // calls constructor 3
        this(5, 15);
        System.out.println(x);
    }

    // parameterized constructor 3
    Temp(int x, int y)
    {
        System.out.println(x * y);
    }

    public static void main(String args[])
    {
        // invokes default constructor first
        new Temp();
    }
}
```


**Rules of constructor chaining:**
1. The this() expression should always be the first line of the constructor.
2. There should be at-least be one constructor without the this() keyword (constructor 3 in above example).
3. Constructor chaining can be achieved in any order.

**2. From base class**

by using super() keyword to call the constructor from the base class.

Here constructor chaining occurs through inheritance. A sub-class constructor's task is to call super class's constructor first. This ensures that the creation of sub class's object starts with the initialization of the data members of the superclass. There could be any number of classes in the inheritance chain. Every constructor calls up the chain till the class at the top is reached.

```java
// Java program to illustrate Constructor Chaining
// within same class Using this() keyword
class Temp
{
    // default constructor 1
    // default constructor will call another constructor
    // using this keyword from same class
    Temp()
    {
        // calls constructor 2
        this(5);
        System.out.println("The Default constructor");
    }

    // parameterized constructor 2
    Temp(int x)
    {
        // calls constructor 3
        this(5, 15);
        System.out.println(x);
    }

    // parameterized constructor 3
    Temp(int x, int y)
    {
        System.out.println(x * y);
    }

    public static void main(String args[])
    {
        // invokes default constructor first
        new Temp();
    }
}
```

**Rules of constructor chaining:**

1. The this() expression should always be the first line of the constructor.
2. There should be at-least be one constructor without the this() keyword (constructor 3 in above example).
3. Constructor chaining can be achieved in any order.

**One more example:**

```java
class Base
{
    Base()
    {
        System.out.println("Base constructor called");
    }
}

class Derived extends Base
{
    // constructor 3
    Derived()
    {
        System.out.println("Derived constructor called");
    }

    public static void main(String args[])
    {

       Derived d = new Derived();
    }
}
// Output:
// Base constructor called
// Derived constructor called
```



