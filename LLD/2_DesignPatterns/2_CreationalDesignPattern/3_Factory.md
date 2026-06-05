## Factory Design Pattern
1. Provides a way to create objects without exposing the creation logic to the client.
2. Refers to the newly created objects using a common interface.
3. Helps achieve loose coupling between client code and object creation.

```java

interface Shape
{
    void draw();
}

class Circle implements Shape
{
    @Override
    public void draw()
    {
        System.out.println("Drawing circle");
    }
}

class Rectangle implements Shape
{
    @Override
    public void draw()
    {
        System.out.println("Drawing rectangle");
    }
}

class ShapeFactory
{
    Shape getShape(String input)
    {
        switch(input)
        {
            case "circle": return new Circle();
            case "rectangle": return new Rectangle();
        }

        return null;
    }
}

class FactoryPattern
{
    public static void main(String[] args)
    {
        ShapeFactory sf = new ShapeFactory();
        Shape s1 = sf.getShape("circle");
    }
}

```