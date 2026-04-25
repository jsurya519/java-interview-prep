## Static Method:
A static method belongs to the class rather than any specific object, allowing it to be called without creating an instance. It is commonly used for operations that do not depend on object state.

1. Can be called using the class name without creating an object
2. Can access only static variables and methods
3. Cannot directly access non-static (instance) members

```java
import java.io.*;

class Geeks {

    // static method
    public static void greet() {

        System.out.println("Hello Geek!");
    }
    public static void main(String[] args) {

        // calling the method directily
        greet();

        // using the class name
        Geeks.greet();
    }
}
// Output:
// Hello Geek!
// Hello Geek!

```

```java
class Main {
    static int staticVar = 10;
    int nonStaticVar = 20;

    static void staticMethod() {
        // Direct access to static field ✅
        System.out.println("Static var: " + staticVar);

        // ❌ Cannot directly access non-static field
        // System.out.println(nonStaticVar); // Compile error

        // ✅ Access non-static field using object
        Main obj = new Main();
        System.out.println("Non-static var: " + obj.nonStaticVar);
    }

    public static void main(String[] args) {
        staticMethod();
    }
}
// Output:
// Static var: 10
// Non-static var: 20
```


---

## Instance Method:

An instance method belongs to an object of a class and is used to operate on object-specific data. It requires creating an instance of the class to be called.

1. Can access instance variables, other instance methods, and static members
2. Requires an object to invoke the method
3. Has access to the this reference, pointing to the current object

```java
import java.io.*;

class Test {
    String n = "";

    // Instance method
    public void test(String n) {
      this.n = n;
    }
}
class Geeks {
    public static void main(String[] args) {

        // create an instance of the class
        Test t = new Test();

        // calling an instance method in the class 'Geeks'
        t.test("GeeksforGeeks");
        System.out.println(t.n);
    }
}

// Output:
// GeeksforGeeks
```

---

## Static vs Instance:

| Features   | Static method | Instance method     |
|--------|-----|----------|
| Definition   | Created using the static keyword and retrieved without creating an object.  | Requires an object of its class to be invoked.  |
| Access  | Access only static variables and methods.  | Can access both static and instance members.|
| this keyword   | Cannot use the this keyword within static methods.  | Can use the this keyword to refer to the current object.|
| Override   | Does not support runtime polymorphism  | Supports runtime polymorphism|
| Memory Allocation   | Loaded once per class  | Each object has its own copy|