## Constructor:
A constructor in Java is a special member that is called when an object is created.
1. A constructor has the same name as the class.
2. It does not have a return type, not even void.
3. It can accept parameters to initialize object properties.

```java
class Student {
    String name;

    // Constructor
    Student(String name) {
        this.name = name;
    }

    void display() {
        System.out.println("Name: " + name);
    }

    public static void main(String[] args) {
        Student s1 = new Student("Vishnu");
        s1.display();
    }
}
```

## Types of Constructors
1. Default Constructor
2. Parameterized Constructor
3. Private Constructor
4. Copy Constructor

**1. Default Constructor**
A default constructor has no parameters. It’s used to assign default values to an object. If no constructor is explicitly defined, Java provides a default constructor.

```java
import java.io.*;

class Geeks{

    // Default Constructor
    Geeks(){

        System.out.println("Default constructor");

    }
    public static void main(String[] args){

        Geeks hello = new Geeks();
    }
}
```

**2. Parameterized Constructor**
A constructor that has parameters is known as parameterized constructor. If we want to initialize fields of the class with our own values, then use a parameterized constructor.

```java
class Geeks{

    // data members of the class
    String name;
    int id;

    // Parameterized Constructor
    Geeks(String name, int id)
    {
        this.name = name;
        this.id = id;
    }

    // Method to display object data
    void display(){

        System.out.println("GeekName: " + name
                           + " and GeekId: " + id);
    }

    // main() method — placed inside the same class for
    // universal compatibility
    public static void main(String[] args){

        // This will invoke the parameterized constructor
        Geeks geek1 = new Geeks("Sweta", 68);
        geek1.display();
    }
}
```

**3. Copy Constructor**
Copy constructor is a constructor for the class that takes an instance of the same class as its argument. This constructor will be used to create a new copy of the object.

```java
import java.io.*;

class Geeks{

    // data members of the class
    String name;
    int id;

    // Parameterized Constructor
    Geeks(String name, int id)
    {
        this.name = name;
        this.id = id;
    }

    // Copy Constructor
    Geeks(Geeks obj2)
    {
        this.name = obj2.name;
        this.id = obj2.id;
    }
}

class GFG {
    public static void main(String[] args)
    {
        // This would invoke the parameterized constructor
        System.out.println("First Object");
        Geeks geek1 = new Geeks("Sweta", 68);
        System.out.println("GeekName: " + geek1.name
                           + " and GeekId: " + geek1.id);

        System.out.println();

        // This would invoke the copy constructor
        Geeks geek2 = new Geeks(geek1);
        System.out.println(
            "Copy Constructor used Second Object");
        System.out.println("GeekName: " + geek2.name
                           + " and GeekId: " + geek2.id);
    }
}
```

**4. Private Constructor**
A private constructor cannot be accessed from outside the class. It is used to prevent instantiation of a class from outside and allows instantiation only from static methods inside the class.

```java
class Gfg
{
    String name;
    private Gfg(String name)
    {
        this.name=name;
    }

    public static Gfg createGfg(String name)
    {
        return new Gfg(name);
    }
}

public class Main {

    public static void main(String[] args) {

        // Private constructor can't be called;
        //Gfg g = new Gfg("geeks");

        Gfg g = Gfg.createGfg("geeks");
        System.out.println(g.name);
    }
}

```
