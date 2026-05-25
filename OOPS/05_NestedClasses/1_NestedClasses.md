## Nested classes:
Defining a class within another class known as nested classes.
1. A nested class is also a member of its enclosing class.
2. As a member of its enclosing class, a nested class can be declared private, public, protected, or package-private(default).
3. The scope of a nested class is bounded by the scope of its enclosing class.
4. A nested class has access to the members, including private members, of the class in which it is nested.

## Nested classes are divided into two categories:
1. **inner class:** An inner class is a non-static nested class.
2. **static nested class:** Nested classes that are declared static are called static nested classes.
---
## Inner class:

**case 1: Inner Class Requires Outer Object**
```java
class A {

    class B {

        void show() {
            System.out.println("Inside B");
        }
    }

    public static void main(String[] args) {

        A objA = new A();

        // Creating inner class object
        A.B objB = objA.new B();

        objB.show();
    }
}
// Output:
// Inside B
```

**case 2: Inner Class Can Access Private Members of Outer Class**
```java
class A {

    private int x = 100;

    class B {

        void display() {
            System.out.println(x);
        }
    }

    public static void main(String[] args) {

        A objA = new A();
        A.B objB = objA.new B();

        objB.display();
    }
}
// Output:
// 100
```

**case 3: Outer Class Cannot Directly Access Inner Members**
**Wrong way:**
```java
class A {

    class B {
        int y = 50;
    }

    void show() {

        // ERROR
        // System.out.println(y);
    }
}
```

**Correct way:**
```java
class A {

    class B {
        int y = 50;
    }

    void show() {

        B obj = new B();

        System.out.println(obj.y);
    }

    public static void main(String[] args) {

        A obj = new A();

        obj.show();
    }
}
// Output:
// 50
```

**case 4: Static Nested Class Does Not Need Outer Object**
```java
class A {

    static class B {

        void show() {
            System.out.println("Static Nested Class");
        }
    }

    public static void main(String[] args) {

        A.B obj = new A.B();

        obj.show();
    }
}
// Output:
// Static Nested Class
```

**case 5: Static Nested Class Cannot Access Outer Instance Variable**
Wrong Way
```java
class A {

    int x = 10;

    static class B {

        void show() {

            // ERROR
            // System.out.println(x);
        }
    }
}
```

Correct Way Using Object
```java
class A {

    int x = 10;

    static class B {

        void show() {

            A obj = new A();

            System.out.println(obj.x);
        }
    }

    public static void main(String[] args) {

        A.B obj = new A.B();

        obj.show();
    }
}
// Output:
// 10
```

**case 6:. Static Nested Class Can Access Static Members Directly**
```java
class A {

    static int x = 100;

    static class B {

        void show() {
            System.out.println(x);
        }
    }

    public static void main(String[] args) {

        A.B obj = new A.B();

        obj.show();
    }
}
// Output:
// 100
```

**case 7: Static Members Can Be Accessed Without Object**
```java
class A {

    static class B {

        static int x = 500;

        static void test() {
            System.out.println("Static Method");
        }
    }

    public static void main(String[] args) {

        System.out.println(A.B.x);

        A.B.test();
    }
}
// Output:
// 500
// Static Method
```

**case 8: Deep Nested Static Classes**
```java
class A {

    static class B {

        static class C {

            static int x = 999;
        }
    }

    public static void main(String[] args) {

        System.out.println(A.B.C.x);
    }
}
// Output:
// 999
```

**case 9: Non-Static Inner Class Accessing Outer Variable**
```java
class A {

    int x = 10;

    class B {

        void increment() {

            x++;

            System.out.println(x);
        }
    }

    public static void main(String[] args) {

        A objA = new A();

        A.B objB = objA.new B();

        objB.increment();
    }
}
// Output:
// 11
```

**case 10: Static Nested Class Used as Helper Class**
```java
class Car {

    static class Engine {

        void start() {
            System.out.println("Engine Started");
        }
    }

    public static void main(String[] args) {

        Car.Engine e = new Car.Engine();

        e.start();
    }
}
// Output:
// Engine Started
```


---

```java
// Java program to demonstrate accessing
// a static nested class

// outer class
class OuterClass {
    // static member
    static int outer_x = 10;

    // instance(non-static) member
    int outer_y = 20;

    // private member
    private static int outer_private = 30;

    // static nested class
    static class StaticNestedClass {
        void display()
        {
            // can access static member of outer class
            System.out.println("outer_x = " + outer_x);

            // can access private static member of
            // outer class
            System.out.println("outer_private = "
                               + outer_private);

            // The following statement will give compilation
            // error as static nested class cannot directly
            // access non-static members
            // System.out.println("outer_y = " + outer_y);

              // Therefore create object of the outer class
              // to access the non-static member
              OuterClass out = new OuterClass();
              System.out.println("outer_y = " + out.outer_y);


        }
    }
}

// Driver class
public class StaticNestedClassDemo {
    public static void main(String[] args)
    {
        // accessing a static nested class
        OuterClass.StaticNestedClass nestedObject
            = new OuterClass.StaticNestedClass();

        nestedObject.display();
    }
}
// Output:
// outer_x = 10
// outer_private = 30
// outer_y = 20
```

```java
// Java program to demonstrate accessing
// a inner class

// outer class
class OuterClass {
    // static member
    static int outer_x = 10;

    // instance(non-static) member
    int outer_y = 20;

    // private member
    private int outer_private = 30;

    // inner class
    class InnerClass {
        void display()
        {
            // can access static member of outer class
            System.out.println("outer_x = " + outer_x);

            // can also access non-static member of outer
            // class
            System.out.println("outer_y = " + outer_y);

            // can also access a private member of the outer
            // class
            System.out.println("outer_private = "
                               + outer_private);
        }
    }
}

// Driver class
public class InnerClassDemo {
    public static void main(String[] args)
    {
        // accessing an inner class
        OuterClass outerObject = new OuterClass();

        OuterClass.InnerClass innerObject
            = outerObject.new InnerClass();

        innerObject.display();
    }
}
// Output:
// outer_x = 10
// outer_y = 20
// outer_private = 30
```