## Functional Interface:
A functional interface in Java is an interface that has only one abstract method. It can contain default methods, static methods, private methods (Java 9+).
1. Use @FunctionalInterface to ensure only one abstract method (annotation is optional).
2. Enable clean, concise code using lambdas and method references.

**@FunctionalInterface Annotation:**
@FunctionalInterface annotation ensures that an interface cannot have more than one abstract method. If multiple abstract methods are present, the compiler throws an “Unexpected @FunctionalInterface annotation” error.

```java
@FunctionalInterface
interface Square {
    int calculate(int x);
}
```

**Note:**
@FunctionalInterface annotation is optional but it is a good practice to use. It helps catching the error in early stage by making sure that the interface has only one abstract method.

---

## Ways to implement a functional interface locally:

**1. By using local inner class**

```java
@FunctionalInterface
interface Square {
    int calculate(int x);
}

class Main
{
    public static void main(String[] args)
    {
        class Maths implements Square
        {
            @Override
            public int calculate(int x)
            {
                return 0;
            }
        }

        Square s = new Maths();
        s.calculate(10);
    }
}
```


**2. By using anonymous inner class:**
```java
@FunctionalInterface
interface Square {
    int calculate(int x);
}


class Main
{
    public static void main(String[] args)
    {
        Square s = new Square(){
            @Override
            public calculate(int x)
            {
                //definition;
                return 0;
            }
        };

        s.calculate(10);
    }
}
```


**3. Using lambda expression:**
It is used only when the interface is functional interface. But anonymous inner class can be used for all interfaces and classes.

```java
@FunctionalInterface
interface Square {
    int calculate(int x);
}


class Main
{
    public static void main(String[] args)
    {
        Square s = (int x)->{
            //definition
            return 0;
        };

        s.calculate(10);
    }
}
```
