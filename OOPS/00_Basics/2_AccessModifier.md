## Access Modifier

Access modifier defines how the members of a class, like variables, methods, and even the class itself, can be accessed from other parts of our program.


1. private
2. default
3. protected
4. public

**Private Access Modifier**
The private access modifier is specified using the keyword private. The methods or data members declared as private are accessible only within the class in which they are declared.

```java
class Person {

    // private variable
    private String name;

    public void setName(String name)  {

        this.name = name; // accessible within class
    }

    public String getName() { return name; }
}

public class Geeks {
    public static void main(String[] args)
    {

        Person p = new Person();
        p.setName("Alice");

        // System.out.println(p.name); // Error: 'name'
        // has private access
        System.out.println(p.getName());
    }
}

// Output:
// Alice
```

---
**Default Access Modifier**
If no access modifier is specified, the member has default (package-private) access and can only be accessed within the same package.This means only classes within the same package can access it.

```java
class Car {
    String model; // default access
}

public class Main {

    public static void main(String[] args){

        Car c = new Car();
        c.model = "Tesla"; // accessible within the same package
        System.out.println(c.model);
    }
}
// Output:
// Tesla
```

```java
// default access modifier
package p1;

// Class Geek is having
// Default access modifier
class Geek
{
    void display()
    {
        System.out.println("Hello World!");
    }
}


// package with default modifier
package p2;
import p1.*;    // importing package p1

// This class is having
// default access modifier
class GeekNew {
    public static void main(String args[]) {

        // Accessing class Geek from package p1
        Geek o = new Geek();

        o.display();
    }
}


// Output:
// Compile time error
```

---

**Protected Access Modifier**
The protected access modifier is specified using the keyword protected. The methods or data members declared as protected are accessible within the same package or subclasses in different packages.

```java
// File: Vehicle.java in package p1
package p1;
public class Vehicle {
    protected int speed;
}

// File: Bike.java in package p2
package p2;
import p1.Vehicle;
public class Bike extends Vehicle {
    void showSpeed() {
        speed = 100; // allowed: subclass in different package
        System.out.println(speed);
    }
}

// File: Test.java in package p2
package p2;
import p1.Vehicle;
public class Test {
    public static void main(String[] args) {
        Bike b = new Bike();
        b.showSpeed(); // prints 100

        Vehicle v = new Vehicle();
        // System.out.println(v.speed); // ERROR: cannot access protected outside package & non-subclass
    }
}
// Output:
// Access via subclass method: 100
// 0
```

---

**Public Access Modifier**
The public access modifier is specified using the keyword public. Public members are accessible from everywhere in the program. There is no restriction on the scope of public data members.

```java
class MathUtils {

    public static int add(int a, int b) {
        return a + b;
    }
}

public class Main {

    public static void main(String[] args) {

        System.out.println(MathUtils.add(5, 10)); // accessible anywhere
    }
}

// Output:
// 15
```

---

| Access Level                    | Default | Private | Protected | Public |
|--------------------------------|---------|---------|-----------|--------|
| Same Class                     | Yes     | Yes     | Yes       | Yes    |
| Same Package Subclass          | Yes     | No      | Yes       | Yes    |
| Same Package Non-Subclass      | Yes     | No      | Yes       | Yes    |
| Different Package Subclass     | No      | No      | Yes       | Yes    |
| Different Package Non-Subclass | No      | No      | No        | Yes    |