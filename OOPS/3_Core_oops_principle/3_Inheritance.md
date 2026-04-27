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

![0942f7ee8cfe370a3dda34e860a78faa.png](:/9d18995abb1d4ab0968a1ed42745dac2)

## 1. Single Inheritance:
![ec8e8bb8c78dcb25716dbab81880af34.png](:/c75995f9fce54a208866151338d20493)

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


![75a7249dff084d95b70c287fa5144503.png](:/897e76d8cc264fb2a6f23fcf84d97d6a)

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
![5ed975c17fdf4a493b2c6231fde3d281.png](:/7f65648f33924abcb2131c7973c405de)

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

![4425382e91bb598e5e0c21337d0d03ca.png](:/4edb3b70bd3a4148974b19be09bd7233)

```java
interface LandVehicle {
    default void landInfo() {
        System.out.println("This is a LandVehicle");
    }
}
interface WaterVehicle {
    default void waterInfo() {
        System.out.println("This is a WaterVehicle");
    }
}
// Subclass implementing both interfaces
class AmphibiousVehicle implements LandVehicle, WaterVehicle {
    AmphibiousVehicle() {
        System.out.println("This is an AmphibiousVehicle");
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

![6c817bf8f918ea1ce8da1bcba5eecd01.png](:/06c5706c54c5451c863cefec49173c74)

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

