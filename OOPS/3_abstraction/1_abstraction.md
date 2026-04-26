## Abstraction:
Abstraction in Java is the process of hiding internal implementation details and showing only essential functionality to the user. It focuses on what an object does rather than how it does it.
Abstract classes may have methods without implementation and must be implemented by subclasses.

## Ways to Achieve Abstraction
1. Abstract class
2. Interface

## Abstract class
An abstract class is a class that cannot be instantiated and may contain abstract methods (methods without body) as well as concrete methods (with implementation).

```java
abstract class TV{

    abstract void turnOn();
    abstract void turnOff();
}

// Concrete class implementing the abstract methods
class TVRemote extends TV{

    @Override
    void turnOn(){

        System.out.println("TV is turned ON.");
    }

    @Override
    void turnOff(){

        System.out.println("TV is turned OFF.");
    }
}

// Main class to demonstrate abstraction
public class Geeks{

    public static void main(String[] args){

        TV remote = new TVRemote();
        remote.turnOn();
        remote.turnOff();
    }
}
```

![3bb4f85f2d4428f5c4d07208c3eb18ee.png](:/3af6ea6e4d2b4cceb75ca1da1a1aec3e)



```java
abstract class Shape{
    String color;

    // these are abstract methods
    abstract double area();
    public abstract String toString();

    // abstract class can have the constructor
    public Shape(String color){

        System.out.println("Shape constructor called");
        this.color = color;
    }

    // this is a concrete method
    public String getColor(){

        return color;

    }
}
class Circle extends Shape{

    double radius;

    public Circle(String color, double radius){

        // calling Shape constructor
        super(color);
        System.out.println("Circle constructor called");
        this.radius = radius;
    }

    @Override double area(){

        return Math.PI * Math.pow(radius, 2);
    }

    @Override public String toString(){

        return "Circle color is " + super.getColor()
            + "and area is : " + area();
    }
}
class Rectangle extends Shape{

    double length;
    double width;

    public Rectangle(String color, double length,
                     double width)
    {
        // calling Shape constructor
        super(color);
        System.out.println("Rectangle constructor called");
        this.length = length;
        this.width = width;
    }

    @Override double area() { return length * width; }

    @Override public String toString()
    {
        return "Rectangle color is " + super.getColor()
            + "and area is : " + area();
    }
}
public class Test {
    public static void main(String[] args)
    {
        Shape s1 = new Circle("Red", 2.2);
        Shape s2 = new Rectangle("Yellow", 2, 4);

        System.out.println(s1.toString());
        System.out.println(s2.toString());
    }
}
```

---

## Interface:
An interface is a blueprint for a class that defines a set of methods a class must implement. It is commonly used to achieve abstraction in Java. Modern Java interfaces can contain abstract methods, constants, and also default or static methods with implementations.

```java
// Define an interface named Shape
interface Shape{

    // Abstract method for calculating the area
    double calculateArea();
}

// Implement the interface
// in a class named Circle
class Circle implements Shape{

    private double r;

    // Constructor for Circle
    public Circle(double r){

      this.r = r;
    }

    // Implementing the abstract method
    // from the Shape interface
    public double calculateArea()
    {
        return Math.PI * r * r;
    }
}

// Implement the interface in a
// class named Rectangle
class Rectangle implements Shape{

    private double length;
    private double width;

    // Constructor for Rectangle
    public Rectangle(double length, double width){

        this.length = length;
        this.width = width;
    }

    // Implementing the abstract
    // method from the Shape interface
    public double calculateArea() {
      return length * width;
    }
}

public class Main {
    public static void main(String[] args) {

        // Reference type is the interface (Shape)
        Shape cir = new Circle(5.0);
        Shape rect = new Rectangle(4.0, 6.0);

        // Dynamic method dispatch — decides which method to call at runtime
        System.out.println("Area of Circle: " + cir.calculateArea());
        System.out.println("Area of Rectangle: " + rect.calculateArea());
    }
}
```