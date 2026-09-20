# Interface Methods in Java

An interface can contain different types of methods.

The important types to remember are:

1. Abstract methods
2. Default methods
3. Static methods
4. Private methods

---

# 1. Abstract Method

An abstract method only declares the method signature.

```java
interface Animal {

    void eat();
}
```

The implementing class must provide the implementation:

```java
class Dog implements Animal {

    @Override
    public void eat() {
        System.out.println("Dog is eating");
    }
}
```

### Important points

```java
interface Animal {

    void eat();
}
```

The following is implicitly true:

```java
public abstract void eat();
```

So these are equivalent:

```java
void eat();
```

and:

```java
public abstract void eat();
```

### Rules

* Abstract interface methods are implicitly `public`.
* They don't have a method body.
* A concrete implementing class must implement them.
* An interface can be implemented by multiple classes.

---

# 2. Default Method

Introduced in **Java 8**.

A default method provides an implementation inside the interface.

```java
interface Animal {

    default void sleep() {
        System.out.println("Animal is sleeping");
    }
}
```

Now the implementing class does **not** have to override it:

```java
class Dog implements Animal {
}
```

```java
Dog dog = new Dog();

dog.sleep();
```

Output:

```text
Animal is sleeping
```

### Why were default methods introduced?

One major reason was **backward compatibility**.

Imagine Java originally had:

```java
interface Payment {

    void pay();
}
```

Many classes already implement it:

```java
class CreditCardPayment implements Payment {
    public void pay() {
        // implementation
    }
}

class UPIPayment implements Payment {
    public void pay() {
        // implementation
    }
}
```

Now suppose we want to add:

```java
void refund();
```

If it were an abstract method:

```java
interface Payment {

    void pay();

    void refund(); // new method
}
```

Every existing implementation would need to implement `refund()`.

That could break existing implementations.

With a default method:

```java
interface Payment {

    void pay();

    default void refund() {
        System.out.println("Default refund");
    }
}
```

Existing implementations continue working.

---

# 3. Static Method

Interfaces can have static methods.

```java
interface Utility {

    static void printMessage() {
        System.out.println("Hello");
    }
}
```

Call it using the **interface name**:

```java
Utility.printMessage();
```

### Important

You cannot call it through an implementing object:

```java
class Test implements Utility {
}
```

This is NOT valid:

```java
Test test = new Test();

test.printMessage(); // ❌
```

Use:

```java
Utility.printMessage(); // ✅
```

### Why?

Static methods belong to the **interface itself**, not to implementing objects.

---

# 4. Private Method

Introduced in **Java 9**.

An interface can contain private methods to share common implementation between its default and static methods.

```java
interface Payment {

    default void pay() {
        validate();
        System.out.println("Processing payment");
    }

    default void refund() {
        validate();
        System.out.println("Processing refund");
    }

    private void validate() {
        System.out.println("Validating payment");
    }
}
```

Here:

```text
pay()
   ↓
validate()

refund()
   ↓
validate()
```

Both default methods reuse the common logic.

### Important

Private interface methods:

* Cannot be accessed by implementing classes.
* Cannot be overridden.
* Are only accessible inside the interface.
* Can have a method body.
* Can be used by default and static methods.

---

# 5. Can an Interface Have All Four?

Yes.

```java
interface Payment {

    // Abstract
    void pay();

    // Default
    default void refund() {
        validate();
        System.out.println("Refund");
    }

    // Static
    static void info() {
        System.out.println("Payment interface");
    }

    // Private
    private void validate() {
        System.out.println("Validating");
    }
}
```

---

# 6. Comparison

| Method   | Body? | Implicit modifier | Called through      | Overridable?   |
| -------- | ----- | ----------------- | ------------------- | -------------- |
| Abstract | ❌     | `public abstract` | Implementing object | Must implement |
| Default  | ✅     | `public`          | Object              | ✅ Yes          |
| Static   | ✅     | `public`          | Interface name      | ❌ No           |
| Private  | ✅     | `private`         | Inside interface    | ❌ No           |

---

# 7. Very Important: Interface Fields

Although this is about methods, remember this common interview point.

Variables declared in an interface are implicitly:

```java
public static final
```

For example:

```java
interface Config {

    int MAX_RETRY = 3;
}
```

is equivalent to:

```java
interface Config {

    public static final int MAX_RETRY = 3;
}
```

Therefore:

```java
Config.MAX_RETRY
```

can be accessed directly.

You cannot modify it:

```java
Config.MAX_RETRY = 5; // ❌
```

---

# 8. Default Method Conflict

This is an important interview question.

Suppose:

```java
interface A {

    default void show() {
        System.out.println("A");
    }
}
```

and:

```java
interface B {

    default void show() {
        System.out.println("B");
    }
}
```

Now:

```java
class Test implements A, B {
}
```

This causes a compilation error because Java doesn't know which default method to use.

```text
A.show()
     \
      → ???
     /
B.show()
```

The implementing class must resolve the conflict:

```java
class Test implements A, B {

    @Override
    public void show() {
        A.super.show();
    }
}
```

Now:

```java
Test test = new Test();

test.show();
```

Output:

```text
A
```

---

# 9. Abstract Class vs Interface Default Method

An important rule:

If a class inherits an abstract method from an interface and also inherits a concrete method with the same signature from a superclass, the class method wins.

Example:

```java
interface A {

    default void show() {
        System.out.println("Interface");
    }
}
```

```java
class Parent {

    public void show() {
        System.out.println("Parent");
    }
}
```

```java
class Child extends Parent implements A {
}
```

Calling:

```java
new Child().show();
```

prints:

```text
Parent
```

### Rule

```text
Class method
    ↓
wins over
    ↓
Interface default method
```

This avoids ambiguity between class inheritance and interface default methods.

---

# 10. Can Default Method Be Private?

No.

A default method is intended to be part of the interface's public contract.

This is invalid:

```java
private default void test() {
}
```

Use:

```java
default void test() {
}
```

or a separate private helper:

```java
private void helper() {
}
```

---

# 11. Can Static Interface Methods Be Overridden?

No.

```java
interface A {

    static void show() {
        System.out.println("A");
    }
}
```

```java
class B implements A {

    // This is NOT overriding A.show()
    static void show() {
        System.out.println("B");
    }
}
```

These are two separate static methods.

Remember:

> **Static methods are hidden, not overridden.**

And interface static methods are accessed through the interface:

```java
A.show();
B.show();
```

---

# 12. Can an Interface Have a Constructor?

No.

```java
interface A {

    A() { } // ❌
}
```

Why?

An interface doesn't represent an object instance that can be constructed directly.

```text
interface
    ↓
contract
    ↓
implemented by class
    ↓
class object gets created
```

---

# 13. Can an Interface Have a Main Method?

Yes.

Because `main()` can be static:

```java
interface Test {

    static void main(String[] args) {
        System.out.println("Hello");
    }
}
```

The JVM can invoke it using:

```text
Test.main(...)
```

---

# 14. Version Timeline

This is useful for interviews:

```text
Java 7 and earlier
        ↓
Interface
    └── abstract methods
    └── public static final fields

Java 8
        ↓
Interface
    ├── abstract methods
    ├── default methods
    └── static methods

Java 9
        ↓
Interface
    ├── abstract methods
    ├── default methods
    ├── static methods
    └── private methods
```

---

# 15. Easy Interview Memory Trick

Think of the four methods like this:

```text
ABSTRACT
    ↓
"I only define WHAT"

DEFAULT
    ↓
"I define WHAT + default HOW"

STATIC
    ↓
"I belong to the INTERFACE"

PRIVATE
    ↓
"I am an INTERNAL HELPER"
```

### One-line interview answer

> **"An interface can contain abstract methods that define the contract, default methods that provide inheritable implementations, static methods that belong to the interface itself, and private methods used internally to share implementation logic within the interface."**

---

# Quick Example Combining Everything

```java
interface Vehicle {

    // Abstract method
    void start();

    // Default method
    default void stop() {
        validate();
        System.out.println("Vehicle stopped");
    }

    // Static method
    static void info() {
        System.out.println("Vehicle interface");
    }

    // Private helper
    private void validate() {
        System.out.println("Validation completed");
    }
}
```

Implementation:

```java
class Car implements Vehicle {

    @Override
    public void start() {
        System.out.println("Car started");
    }
}
```

Usage:

```java
Car car = new Car();

car.start();  // Abstract method implementation
car.stop();   // Default method

Vehicle.info(); // Static method
```

The private `validate()` method cannot be called directly:

```java
car.validate();       // ❌
Vehicle.validate();   // ❌
```

It can only be used internally by the interface.
