## Abstract Factory Design Pattern
1. Provides an interface for creating families of related or dependent objects without specifying their concrete classes.
2. Useful when your system needs to work with multiple types of objects that are related.
3. Helps enforce consistency among related objects.

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

class Triangle implements Shape
{
    @Override
    public void draw()
    {
        System.out.println("Drawing triangle");
    }
}

interface Color
{
    void fill();
}

class Red implements Color
{
    @Override
    public void fill()
    {
        System.out.println("Filling red color");
    }
}

class Green implements Color
{
    @Override
    public void fill()
    {
        System.out.println("Filling Green color");
    }
}

interface AbstractFactory
{
    Shape createShape(String type);
    Color fillColor(String color);
}

class ShapeFactory implements AbstractFactory
{
    @Override
    public Shape createShape(String type)
    {
        switch(type)
        {
            case "circle": return new Circle();
            case "triangle": return new Triangle();
        }

        return null;
    }

    @Override
    public Color fillColor(String type)
    {
        return null;
    }
}

class ColorFactory implements AbstractFactory
{
    @Override
    public Shape createShape(String type)
    {
        return null;
    }

    @Override
    public Color fillColor(String type)
    {
        switch(type)
        {
            case "red": return new Red();
            case "green": return new Green();
        }

        return null;
    }
}

class Factory
{
    AbstractFactory getFactory(String type)
    {
        switch(type)
        {
            case "shape": return new ShapeFactory();
            case "color": return new ColorFactory();
        }

        return null;
    }
}

class AbstractFactoryPattern
{
    public static void main(String[] args)
    {
        Factory factory = new Factory();
        AbstractFactory af = factory.getFactory("shape");
        Shape circle = af.createShape("circle");
        circle.draw();
    }
}

```