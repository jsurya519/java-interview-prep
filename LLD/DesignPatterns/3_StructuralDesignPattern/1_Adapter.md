## Adapter Design Pattern
Adapter Design Pattern is a structural pattern that acts as a bridge between two incompatible interfaces, allowing them to work together. It is especially useful for integrating legacy code or third-party libraries into a new system.

```java
interface ModernPrinter
{
    void print(String doc);
}

class LegacyPrinter
{
    void printDocument(String doc, int status)
    {
        if(status == 1)
        {
            System.out.println("print document");
        }
        else
        {
            System.out.println("system error");
        }
    }
}

class PrinterAdapter implements ModernPrinter
{

    private LegacyPrinter lp;

    PrinterAdapter(LegacyPrinter p)
    {
        this.lp=p;
    }

    @Override
    public void print(String doc)
    {
        this.lp.printDocument(doc, 1);
    }
}

class AdapterPattern
{
    public static void main(String[] args)
    {
        ModernPrinter mp = new PrinterAdapter(new LegacyPrinter());
        mp.print("Hello World!");
    }
}

```