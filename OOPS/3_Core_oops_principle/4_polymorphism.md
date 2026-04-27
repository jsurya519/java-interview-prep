## Polymorphism:
poly - many
morph - forms
Means it has multiple forms. It allows objects to behave differently based on their specific class type.

## Types of Polymorphism:
<img width="800" height="400" alt="image" src="https://github.com/user-attachments/assets/6b49fa0d-9db1-4a97-9118-829f30384639" />


## 1. Compile-Time Polymorphism
Compile-Time Polymorphism in Java, also known as static polymorphism, is achieved mainly through method overloading.

**Method Overloading:**
Method overloading means defining multiple methods with the same name but different parameter lists. The method call is resolved at compile time based on the arguments passed.

```java
class Helper {

    // Method with 2 integer parameters
    static int Multiply(int a, int b)
    {
        // Returns product of integer numbers
        return a * b;
    }

    // Method 2
    // With same name but with 2 double parameters
    static double Multiply(double a, double b)
    {
        // Returns product of double numbers
        return a * b;
    }
}


// Main class
class Geeks {
    // Main driver method
    public static void main(String[] args)
    {

        // Calling method by passing
        // input as in arguments
        System.out.println(Helper.Multiply(2, 4));
        System.out.println(Helper.Multiply(5.5, 6.3));
    }
}
// Output:
// 8
// 34.65
```

## 2. Runtime Polymorphism
Runtime Polymorphism in Java is also known as dynamic method dispatch. It occurs when a method call is resolved at runtime, and it is achieved using method overriding.

**Method Overriding**
Method overriding occurs when a subclass provides its own implementation of a method already defined in its superclass. The method must have the same name, parameters, and return type.

**Note:** At runtime, the method that gets executed depends on the actual object type, not the reference type.

```java
// Class 1
// Helper class
class Parent {

    // Method of parent class
    void Print() { System.out.println("parent class"); }
}

// Class 2
// Helper class
class Subclass1 extends Parent {

    // Method
    void Print() { System.out.println("subclass1"); }
}

// Class 3
// Helper class
class Subclass2 extends Parent {

    // Method
    void Print() { System.out.println("subclass2"); }
}

// Class 4
// Main class
class Geeks {

    // Main driver method
    public static void main(String[] args)
    {

        // Creating object of class 1
        Parent a;

        // Now we will be calling print methods
        // inside main() method
        a = new Subclass1();
        a.Print();

        a = new Subclass2();
        a.Print();
    }
}
// Output:
// subclass1
// subclass2
```


```java
class Animal {
    void makeSound() {
        System.out.println("Animal makes a sound");
    }
}

class Dog extends Animal {
    @Override
    void makeSound() {
        System.out.println("Dog barks");
    }
}

class Cat extends Animal {
    @Override
    void makeSound() {
        System.out.println("Cat meows");
    }
}

public class Main {
    public static void main(String[] args) {

        Animal a;  // reference of parent

        boolean condition = Math.random() > 0.5;

        if (condition) {
            a = new Dog();
            System.out.println("Object created is Dog");
        } else {
            a = new Cat();
            System.out.println("Object created is Cat");
        }

        a.makeSound();  // 🔥 runtime polymorphism happens here
    }
}
```
