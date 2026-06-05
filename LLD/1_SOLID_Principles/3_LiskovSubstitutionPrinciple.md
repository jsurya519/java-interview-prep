## Liskov's Substitution Principle
**"Derived or child classes must be substitutable for their base or parent classes"**. This principle ensures that any class that is the child of a parent class should be usable in place of its parent without any unexpected behaviour.

```java
class Rectangle
{
    private double length;
    private double breadth;

    Rectangle(double length, double breadth)
    {
        this.length = length;
        this.breadth = breadth;
    }

    double calculateArea()
    {
        return length * breadth;
    }
}

class Square extends Rectangle
{
    Square(double side)
    {
        super(side, side);
    }
}

public class Main
{
    static void printArea(Rectangle r)
    {
        System.out.println("Area = " + r.calculateArea());
    }

    public static void main(String[] args)
    {
        Rectangle rectangle = new Rectangle(4, 5);
        Rectangle square = new Square(5);

        printArea(rectangle);
        printArea(square);
    }
}
```