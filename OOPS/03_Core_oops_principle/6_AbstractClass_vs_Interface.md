# Abstract Class vs Interface

Both **abstract classes** and **interfaces** are used to achieve abstraction, but they serve different design purposes.

The simplest mental model:

> **Abstract class = "What an object IS" + shared state/behavior**

> **Interface = "What an object CAN DO" / a contract**

---

# 1. Basic Example

## Abstract Class

Suppose we have different types of employees.

```java
abstract class Employee {

    private String name;

    public Employee(String name) {
        this.name = name;
    }

    public void login() {
        System.out.println(name + " logged in");
    }

    public abstract void work();
}
```

Then:

```java
class Developer extends Employee {

    public Developer(String name) {
        super(name);
    }

    @Override
    public void work() {
        System.out.println("Writing code");
    }
}
```

Here:

```text
Employee
    ↑
    |
Developer
```

`Developer IS AN Employee`.

This is a natural use case for an abstract class.

---

# 2. Interface

Now imagine capabilities:

```java
interface Flyable {

    void fly();
}
```

```java
interface Swimmable {

    void swim();
}
```

A class can implement both:

```java
class Duck implements Flyable, Swimmable {

    @Override
    public void fly() {
        System.out.println("Duck is flying");
    }

    @Override
    public void swim() {
        System.out.println("Duck is swimming");
    }
}
```

Here:

```text
             Duck
            /    \
           ↓      ↓
       Flyable  Swimmable
```

The interfaces represent **capabilities/contracts**.

---

# 3. Major Difference: Multiple Inheritance

This is one of the biggest differences.

Java allows a class to extend only **one class**:

```java
class Developer extends Employee {
}
```

This is valid.

But:

```java
class Developer extends Employee, Person {
}
```

is invalid.

```text
❌ Java does not support multiple class inheritance.
```

However, a class can implement multiple interfaces:

```java
class Duck implements Flyable, Swimmable, Runnable {
}
```

```text
✅ Multiple interfaces are allowed.
```

This is one major reason interfaces are useful for defining capabilities.

---

# 4. Methods

## Abstract Class

An abstract class can have:

* Abstract methods
* Concrete methods

```java
abstract class Employee {

    public abstract void work();

    public void login() {
        System.out.println("Login");
    }
}
```

---

## Interface

Modern Java interfaces can have:

* Abstract methods
* Default methods
* Static methods
* Private methods

```java
interface Employee {

    void work();

    default void login() {
        System.out.println("Login");
    }

    static void info() {
        System.out.println("Employee interface");
    }

    private void validate() {
        System.out.println("Validation");
    }
}
```

---

# 5. State / Instance Variables

This is a **very important difference**.

An abstract class can have instance variables:

```java
abstract class Employee {

    protected String name;
    protected int salary;
}
```

Each object gets its own state:

```text
Developer object
    |
    ├── name
    └── salary
```

An interface cannot have instance variables.

Interface fields are implicitly:

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

So an interface is not meant to hold per-object mutable state.

---

# 6. Constructors

## Abstract Class

Can have constructors:

```java
abstract class Employee {

    protected String name;

    public Employee(String name) {
        this.name = name;
    }
}
```

The subclass constructor can invoke it:

```java
class Developer extends Employee {

    public Developer(String name) {
        super(name);
    }
}
```

---

## Interface

Cannot have constructors:

```java
interface Employee {

    Employee() { } // ❌
}
```

Because you cannot directly create an interface object.

```java
new Employee(); // ❌
```

---

# 7. Access Modifiers

Abstract class methods and fields can have normal Java access modifiers:

```java
abstract class Employee {

    private int salary;

    protected void calculateSalary() {
    }

    public void login() {
    }

    void workInternally() {
    }
}
```

Interfaces have more restrictions.

Abstract interface methods are implicitly:

```java
public abstract
```

For example:

```java
interface Employee {

    void work();
}
```

means:

```java
interface Employee {

    public abstract void work();
}
```

Modern interfaces can additionally contain `default`, `static`, and `private` methods.

---

# 8. Can Abstract Class Have Static Methods?

Yes.

```java
abstract class Employee {

    static void companyInfo() {
        System.out.println("Company");
    }
}
```

Can be called:

```java
Employee.companyInfo();
```

Similarly, interfaces can have static methods:

```java
interface Employee {

    static void companyInfo() {
        System.out.println("Company");
    }
}
```

```java
Employee.companyInfo();
```

---

# 9. Can Abstract Class Have Final Methods?

Yes.

```java
abstract class Employee {

    public final void login() {
        System.out.println("Login");
    }
}
```

A subclass cannot override it:

```java
class Developer extends Employee {

    // ❌ Cannot override final method
}
```

This is useful when the parent wants to provide behavior that subclasses **must not change**.

---

# 10. Can Interface Methods Be Final?

Interface methods cannot be declared `final`.

For example:

```java
interface Employee {

    final void work(); // ❌
}
```

doesn't make sense because interface abstract methods are meant to be implemented by classes.

---

# 11. Can Abstract Class Implement Interface?

Yes.

This is actually very common.

```java
interface Payment {

    void pay();
}
```

```java
abstract class OnlinePayment implements Payment {

    public void validatePayment() {
        System.out.println("Validation");
    }

    // pay() can remain abstract
}
```

Then:

```java
class CreditCardPayment extends OnlinePayment {

    @Override
    public void pay() {
        System.out.println("Credit card payment");
    }
}
```

Hierarchy:

```text
Payment (interface)
       ↑
       |
OnlinePayment (abstract class)
       ↑
       |
CreditCardPayment
```

---

# 12. Can an Interface Extend Another Interface?

Yes.

```java
interface Animal {

    void eat();
}
```

```java
interface Pet extends Animal {

    void play();
}
```

Then:

```java
class Dog implements Pet {

    @Override
    public void eat() {
    }

    @Override
    public void play() {
    }
}
```

An interface can extend **multiple interfaces**:

```java
interface Pet extends Animal, Runnable {
}
```

---

# 13. Can an Abstract Class Extend Another Abstract Class?

Yes.

```java
abstract class Animal {

    abstract void eat();
}
```

```java
abstract class Mammal extends Animal {

    abstract void walk();
}
```

```java
class Dog extends Mammal {

    @Override
    void eat() {
    }

    @Override
    void walk() {
    }
}
```

---

# 14. Object Creation

Neither can normally be instantiated directly.

Abstract class:

```java
abstract class Animal {
}
```

```java
Animal animal = new Animal(); // ❌
```

Interface:

```java
interface Animal {
}
```

```java
Animal animal = new Animal(); // ❌
```

But both can be used as reference types:

```java
Employee employee = new Developer();
```

and:

```java
Flyable flyable = new Duck();
```

This is important for **polymorphism**.

---

# 15. Multiple Inheritance Comparison

### Abstract class

```text
        Employee
           ↑
           |
       Developer
```

Only one parent class:

```java
class Developer extends Employee {
}
```

Cannot:

```java
class Developer extends Employee, Person { } // ❌
```

### Interface

```text
       Flyable     Swimmable
          ↑           ↑
           \         /
              Duck
```

```java
class Duck implements Flyable, Swimmable {
}
```

Multiple interfaces are allowed.

---

# 16. Why Doesn't Java Allow Multiple Class Inheritance?

One major problem is ambiguity.

Imagine:

```java
class A {

    void show() {
        System.out.println("A");
    }
}
```

```java
class B {

    void show() {
        System.out.println("B");
    }
}
```

If Java allowed:

```java
class C extends A, B {
}
```

What should happen?

```java
new C().show();
```

Should it call:

```text
A.show()
```

or:

```text
B.show()
```

This is one of the classic problems associated with multiple class inheritance.

Java avoids this by allowing:

```text
one superclass
+
multiple interfaces
```

---

# 17. But Interfaces Can Also Have Default Method Conflicts

Modern interfaces can have implementation through default methods.

For example:

```java
interface A {

    default void show() {
        System.out.println("A");
    }
}
```

```java
interface B {

    default void show() {
        System.out.println("B");
    }
}
```

Now:

```java
class C implements A, B {
}
```

This causes a compilation error because Java cannot choose between `A.show()` and `B.show()`.

The class must resolve it:

```java
class C implements A, B {

    @Override
    public void show() {
        A.super.show();
    }
}
```

So Java provides explicit rules to resolve default-method conflicts.

---

# 18. When Should I Use Abstract Class?

Use an abstract class when classes have a **strong "IS-A" relationship** and you want to share:

* State
* Common behavior
* Constructors
* Protected methods
* Common implementation

Example:

```text
Vehicle
   ↑
   ├── Car
   ├── Bike
   └── Truck
```

A `Car IS A Vehicle`.

An abstract class makes sense:

```java
abstract class Vehicle {

    protected String registrationNumber;

    public Vehicle(String registrationNumber) {
        this.registrationNumber = registrationNumber;
    }

    public void startEngine() {
        System.out.println("Engine started");
    }

    public abstract void drive();
}
```

---

# 19. When Should I Use Interface?

Use an interface when you want to define a **contract/capability** that potentially unrelated classes can implement.

For example:

```java
interface Payable {
    void pay();
}
```

Different classes can implement it:

```java
class Employee implements Payable {
}

class Invoice implements Payable {
}

class Order implements Payable {
}
```

These classes don't necessarily have a common parent conceptually, but they all have the capability:

```text
            Payable
           /   |   \
          /    |    \
    Employee Invoice Order
```

---

# 20. Real-World Example

Suppose you're designing a payment system.

You might have:

```java
interface PaymentProcessor {

    void processPayment();
}
```

Different implementations:

```java
class StripePaymentProcessor
        implements PaymentProcessor {

    @Override
    public void processPayment() {
        // Stripe logic
    }
}
```

```java
class RazorpayPaymentProcessor
        implements PaymentProcessor {

    @Override
    public void processPayment() {
        // Razorpay logic
    }
}
```

The interface defines the contract:

```text
PaymentProcessor
       |
       ├── StripePaymentProcessor
       |
       └── RazorpayPaymentProcessor
```

Your business logic can depend on:

```java
PaymentProcessor processor;
```

rather than a specific implementation.

This gives you **loose coupling** and makes implementations easier to replace or test.

---

# 21. Quick Comparison Table

| Feature                        | Abstract Class          | Interface                          |
| ------------------------------ | ----------------------- | ---------------------------------- |
| Can have abstract methods      | ✅                       | ✅                                  |
| Can have concrete methods      | ✅                       | ✅ (`default`, `static`, `private`) |
| Can have instance variables    | ✅                       | ❌                                  |
| Can have `static final` fields | ✅                       | ✅                                  |
| Can have constructors          | ✅                       | ❌                                  |
| Can be instantiated directly   | ❌                       | ❌                                  |
| Multiple inheritance           | ❌                       | ✅ Multiple interfaces              |
| Can extend class               | ✅                       | ❌                                  |
| Can extend interfaces          | ❌                       | ✅                                  |
| Can implement interfaces       | ✅                       | N/A                                |
| Can have `private` methods     | ✅                       | ✅ Since Java 9                     |
| Can have `protected` methods   | ✅                       | ❌                                  |
| Can have `final` methods       | ✅                       | ❌                                  |
| Main purpose                   | Shared state + behavior | Contract / capability              |

---

# 22. The Most Important Interview Difference

Don't just say:

> "Abstract class can have concrete methods, interface cannot."

That answer is **outdated** because modern interfaces can have `default`, `static`, and `private` methods.

Instead say:

> **"The fundamental difference is that an abstract class is useful when we have a common base type where we want to share state, constructors, and implementation, while an interface is primarily used to define a contract or capability that can be implemented by multiple, potentially unrelated classes. A class can extend only one class but can implement multiple interfaces."**

That's a much stronger answer for a senior-level interview.

---

# 23. Easy Mental Model

Remember:

```text
ABSTRACT CLASS
       ↓
Common identity
       ↓
"IS-A"
       ↓
Shared state + behavior
       ↓
Single inheritance
```

```text
INTERFACE
       ↓
Contract / capability
       ↓
"CAN-DO"
       ↓
Multiple interfaces
       ↓
Loose coupling
```

### Example

```text
Car
 |
 └── IS-A → Vehicle
             ↓
        abstract class


Car
 |
 ├── CAN-DO → Serializable
 ├── CAN-DO → Comparable
 └── CAN-DO → ElectricVehicle
             ↓
          interfaces
```

---

# 24. Interview Follow-Up Questions to Prepare

After this question, an interviewer may ask:

1. **Why did Java 8 introduce default methods?**
2. **Why can interfaces have static methods but they cannot be overridden?**
3. **What happens if two interfaces have the same default method?**
4. **What happens if a superclass and interface both have the same method?**
5. **Can an abstract class implement an interface without implementing all methods?**
6. **Can an interface extend multiple interfaces?**
7. **Why doesn't Java support multiple class inheritance?**
8. **What is the difference between abstraction and encapsulation?**
9. **When would you choose an abstract class over an interface in a real application?**
10. **Can an interface have variables? What are their implicit modifiers?**

These are common follow-ups worth knowing for a Java/Spring senior-level interview.
