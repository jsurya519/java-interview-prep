## Adapter Design Pattern
Adapter Design Pattern is a structural pattern that acts as a bridge between two incompatible interfaces, allowing them to work together. It is especially useful for integrating legacy code or third-party libraries into a new system.

```java
class LegacyPrinter
{
    void printDoc(String s, int status)
    {
        if(status == 1)
            System.out.println(s);
        else
            System.out.println("ERROR PRINTING");
    }
}

interface ModernPrinter
{
    void print(String s);
}

class AdapterPrinter implements ModernPrinter
{
    private final LegacyPrinter lp;

    AdapterPrinter(LegacyPrinter lp)
    {
        this.lp=lp;
    }

    @Override
    public void print(String s)
    {
        this.lp.printDoc(s, 1);
    }
}

class Main
{
    public static void main(String[] args)
    {
        ModernPrinter mp = new AdapterPrinter(new LegacyPrinter());
        mp.print("Hello World!");
    }
}

```