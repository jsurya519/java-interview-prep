## Init Block:
When we want certain common resources to be executed with every constructor we can put the code in the init block. Init block is always executed before any constructor, whenever a constructor is used for creating a new object.

```java
class Base
{
    {
        System.out.println("Base init block");
    }

    Base()
    {
        System.out.println("Base constructor called");
    }
}

class Derived extends Base
{
    {
        System.out.println("derived init block");
    }

    Derived()
    {
        System.out.println("Derived constructor called");
    }

    public static void main(String args[])
    {

       Derived d = new Derived();
    }
}

// output:
// Base init block
// Base constructor called
// derived init block
// Derived constructor called
```

 If there are more than one blocks, they are executed in the order in which they are defined within the same class.

 ```java
class Temp
{
    // block to be executed first
    {
        System.out.println("init");
    }
    Temp()
    {
        System.out.println("default");
    }
    Temp(int x)
    {
        System.out.println(x);
    }

    // block to be executed after the first block
    // which has been defined above.
    {
        System.out.println("second");
    }
    public static void main(String args[])
    {
        new Temp();
        new Temp(10);
    }
}
```
