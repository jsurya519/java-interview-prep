## Inheritance:
Inheritance allows a class to acquire properties and behaviors from another class.
1. A subclass can reuse the fields and methods of the parent class without rewriting the code.
2. A subclass can add its own fields and methods or modify existing ones to extend functionality.

```java
// Parent class
class Animal {
    void sound() {
        System.out.println("Animal makes a sound");
    }
}

// Child class
class Dog extends Animal {
    void sound() {
        System.out.println("Dog barks");
    }
}

// Child class
class Cat extends Animal {
    void sound() {
        System.out.println("Cat meows");
    }
}

// Child class
class Cow extends Animal {
    void sound() {
        System.out.println("Cow moos");
    }
}

// Main class
public class Geeks {
    public static void main(String[] args) {
        Animal a;
        a = new Dog();
        a.sound();

        a = new Cat();
        a.sound();

        a = new Cow();
        a.sound();
    }
}
// Output:
// Dog barks
// Cat meows
// Cow moos
```


## Types of Inheritance:

<img width="1100" height="733" alt="image" src="https://github.com/user-attachments/assets/15c9025b-5b21-4389-952f-ee9d87597701" />


## 1. Single Inheritance:
<img width="682" height="400" alt="image" src="https://github.com/user-attachments/assets/09086734-6e2e-4920-9522-7a6028ef112a" />


```java
//Super class
class Vehicle {
    Vehicle() {
        System.out.println("This is a Vehicle");
    }
}

// Subclass
class Car extends Vehicle {
    Car() {
        System.out.println("This Vehicle is Car");
    }
}

public class Test {
    public static void main(String[] args) {
        // Creating object of subclass invokes base class constructor
        Car obj = new Car();
    }
}
// Output:
// This is a Vehicle
// This Vehicle is Car
```


## 2. Multilevel Inheritance

<img width="682" height="400" alt="image" src="https://github.com/user-attachments/assets/145a54b6-95c5-47dd-8a52-0fce89f02b58" />


```java
class Vehicle {
    Vehicle() {
        System.out.println("This is a Vehicle");
    }
}
class FourWheeler extends Vehicle {
    FourWheeler() {
        System.out.println("4 Wheeler Vehicles");
    }
}
class Car extends FourWheeler {
    Car() {
        System.out.println("This 4 Wheeler Vehicle is a Car");
    }
}
public class Geeks {
    public static void main(String[] args) {
        Car obj = new Car(); // Triggers all constructors in order
    }
}
// Output:
// This is a Vehicle
// 4 Wheeler Vehicles
// This 4 Wheeler Vehicle is a Car
```

## 3. Hierarchical Inheritance
<img width="682" height="400" alt="image" src="https://github.com/user-attachments/assets/4f9abac3-d56d-4a39-8ec8-967b83425727" />


```java
class Vehicle {
    Vehicle() {
        System.out.println("This is a Vehicle");
    }
}

class Car extends Vehicle {
    Car() {
        System.out.println("This Vehicle is Car");
    }
}

class Bus extends Vehicle {
    Bus() {
        System.out.println("This Vehicle is Bus");
    }
}

public class Test {
    public static void main(String[] args) {
        Car obj1 = new Car();
        Bus obj2 = new Bus();
    }
}
// Output:
// This is a Vehicle
// This Vehicle is Car
// This is a Vehicle
// This Vehicle is Bus
```

## 4. Multiple Inheritance (Through Interfaces)
Java does not support multiple inheritances with classes. In Java, we can achieve multiple inheritances only through Interfaces.

<img width="682" height="400" alt="image" src="https://github.com/user-attachments/assets/0fc4f6d5-4792-4f4d-8933-5c347087ba50" />


```java
interface LandVehicle {
    void landInfo();
}
interface WaterVehicle {
    void waterInfo();
}

class AmphibiousVehicle implements LandVehicle, WaterVehicle {
    AmphibiousVehicle() {
        System.out.println("This is an AmphibiousVehicle");
    }

    @Override
    void waterInfo()
    {
        System.out.println("This is a WaterVehicle");

    }

    @Override
    void landInfo()
    {
        System.out.println("This is a landVehicle");

    }
}

public class Test {
    public static void main(String[] args) {
        AmphibiousVehicle obj = new AmphibiousVehicle();
        obj.waterInfo();
        obj.landInfo();
    }
}
// Output:
// This is an AmphibiousVehicle
// This is a WaterVehicle
// This is a LandVehicle
```

## 5. Hybrid Inheritance
It is a mix of two or more of the above types of inheritance. In Java, we can achieve hybrid inheritance only through Interfaces if we want to involve multiple inheritance to implement Hybrid inheritance.

<img width="682" height="400" alt="image" src="https://github.com/user-attachments/assets/33fbc1e8-6af2-4eaf-8b8b-72d75b246084" />


```java
// Superclass
class Vehicle {
    void vehicleType() {
        System.out.println("This is a Vehicle");
    }
}

// Interface for fare
interface Fare {
    default void fareInfo() {
        System.out.println("Fare information");
    }
}

// Subclass 1: Single inheritance + multilevel
class Car extends Vehicle {
    void carType() {
        System.out.println("This is a Car");
    }
}

// Subclass 2: Hybrid inheritance (extends class + implements interface)
class Bus extends Vehicle implements Fare {
    void busType() {
        System.out.println("This is a Bus");
    }
}

public class GFG{
    public static void main(String[] args) {
        Car car = new Car();
        car.vehicleType(); // inherited from Vehicle
        car.carType();     // specific to Car

        Bus bus = new Bus();
        bus.vehicleType(); // inherited from Vehicle
        bus.busType();     // specific to Bus
        bus.fareInfo();    // from Fare interface
    }
}
// Output:
// This is a Vehicle
// This is a Car
// This is a Vehicle
// This is a Bus
// Fare information
```


## Java IS-A type of Relationship
IS-A represents an inheritance relationship in Java, meaning this object is a type of that object.

```java
public class SolarSystem {
}
public class Earth extends SolarSystem {
}
public class Mars extends SolarSystem {
}
public class Moon extends Earth {
}
```

Now, based on the above example, in Object-Oriented terms, the following are true:

SolarSystem is the superclass of Earth class.

SolarSystem is the superclass of Mars class.

Earth and Mars are subclasses of SolarSystem class.

Moon is the subclass of both Earth and SolarSystem classes.

```java
class SolarSystem {
}
class Earth extends SolarSystem {
}
class Mars extends SolarSystem {
}
public class Moon extends Earth {
    public static void main(String args[])
    {
        SolarSystem s = new SolarSystem();
        Earth e = new Earth();
        Mars m = new Mars();

        System.out.println(s instanceof SolarSystem);
        System.out.println(e instanceof Earth);
        System.out.println(m instanceof SolarSystem);
    }
}
// Output:
// true
// true
// true
```

