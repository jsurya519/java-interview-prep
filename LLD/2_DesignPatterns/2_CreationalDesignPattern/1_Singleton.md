## Singleton Design Pattern
The Singleton Design Pattern ensures that only one instance of a class is created throughout the entire application, and provides a global point of access to it.

```java
class Singleton
{
    private static volatile Singleton instance;
    private Singleton()
    {
    }

    public static Singleton getInstance()
    {
        if(instance == null)
        {
            synchronized(Singleton.class)
            {
                if(instance == null)
                {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}

class SingletonPattern
{
    public static void main(String[] args)
    {
        Singleton s = Singleton.getInstance();
    }
}
```