"Derived or child classes must be substitutable for their base or parent classes". This principle ensures that any class that is the child of a parent class should be usable in place of its parent without any unexpected behaviour.

```java
class Rectangle
{
    private double height, width;

    Rectangle(double h, double w)
    {
        this.height=h;
        this.width=w;
    }

    public double area()
    {
        return this.height*this.width;
    }

}

class Square extends Rectangle
{
    Square(double w)
    {
        super(w,w);
    }

}

class LiskovSubstitution
{
    public static void main(String[] args)
    {
        Rectangle sq = new Square(5d);
        System.out.println(sq.area());
    }
}
```