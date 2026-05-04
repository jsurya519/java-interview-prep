## Anonymous Inner class:
 It is an inner class without a name and for which only a single object is created. An anonymous inner class can be useful when making an instance of an object with certain "extras" such as overriding methods of a class or interface, without having to actually subclass a class.

1. Anonymous inner class can implement only one interface at a time.
2.  Anonymous inner class can extend only one class at a time.
3.  Anonymous Inner class can extend a class or can implement an interface but not both at a time.
4.  An anonymous inner class cannot have a constructor.

**Normal Implementation:**
```java

interface Age {
    int x = 21;
    void getAge();
}

class MyClass implements Age {

    // Overriding getAge() method
    @Override
	public void getAge()
    {
        // Print statement
        System.out.print("Age is " + x);
    }
}

class GFG {
    public static void main(String[] args)
    {

        MyClass obj = new MyClass();

        obj.getAge();
    }
}
// Output:
// Age is 21
```

**Demonstrating with Anonymous class:**
```java
// Java Program to Demonstrate Anonymous inner class

// Interface
interface Age {
    int x = 21;
    void getAge();
}

// Main class
class AnonymousDemo {

    // Main driver method
    public static void main(String[] args)
    {

        // A hidden inner class of Age interface is created
        // whose name is not written but an object to it
        // is created.
        Age oj1 = new Age() {

            @Override public void getAge()
            {
                // printing  age
                System.out.print("Age is " + x);
            }
        };

        oj1.getAge();
    }
}
// Output:
// Age is 21
```

## Types of anonymous inner class:
1. Anonymous Inner class that extends a class
2. Anonymous Inner class that implements an interface
3. Anonymous Inner class that defines inside method/constructor argument

**Type 1: Anonymous Inner class that extends a class**

```java
class Parent {

    public int a = 10;
    protected int b = 20;
    int c = 30;          // default
    private int d = 40;

    void display() {
        System.out.println("Parent method");
    }
}

class Test {

    public static void main(String[] args) {

        Parent obj = new Parent() {

            void show() {

                System.out.println(a); // accessible
                System.out.println(b); // accessible
                System.out.println(c); // accessible

                // System.out.println(d); // ERROR
            }
        };

        obj.display();
    }
}
```


**Type 2: Anonymous Inner class that implements an interface**

```java
interface Animal {

    void sound();
}

class Test {

    public static void main(String[] args) {

        Animal a = new Animal() {

            @Override
            public void sound() {
                System.out.println("Dog barks");
            }
        };

        a.sound();
    }
}
```

**Type 3: Anonymous Inner class that defines inside method/constructor argument**

```java
interface Message {

    void send();
}

class Demo {

    void process(Message m) {
        m.send();
    }

    public static void main(String[] args) {

        Demo d = new Demo();

        d.process(new Message() {

            @Override
            public void send() {
                System.out.println("Message Sent");
            }
        });
    }
}
```