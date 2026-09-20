# Spring Boot Interview Preparation

## 1. IoC & Dependency Injection

### 1.1 What is IoC?

**IoC = Inversion of Control**

IoC means that the responsibility for creating and managing objects and their dependencies is moved away from the application classes to an external mechanism such as the **Spring IoC Container**.

### Without IoC

```java
class OrderService {

    private PaymentService paymentService;

    public OrderService() {
        this.paymentService = new PaymentService();
    }
}
```

Here, `OrderService` is responsible for creating its dependency:

```java
new PaymentService()
```

So the control remains inside `OrderService`.

```text
OrderService
     |
     | creates
     v
PaymentService
```

This creates tighter coupling because `OrderService` knows how its dependency is created.

### With IoC

```java
class OrderService {

    private PaymentService paymentService;

    public OrderService(PaymentService paymentService) {
        this.paymentService = paymentService;
    }
}
```

Now `OrderService` does not create `PaymentService`.

It simply says:

> "I need a `PaymentService`; someone else should provide it to me."

The responsibility has moved outside `OrderService`.

That is the basic idea of **Inversion of Control**.

---

## 1.2 How does Spring implement IoC?

Spring provides an **IoC Container** that is responsible for things such as:

- Creating objects (Spring Beans)
- Managing their lifecycle
- Resolving dependencies
- Injecting dependencies
- Managing bean scopes
- Applying configuration and post-processing

For example:

```java
@Service
class PaymentService {
}
```

```java
@Service
class OrderService {

    private final PaymentService paymentService;

    public OrderService(PaymentService paymentService) {
        this.paymentService = paymentService;
    }
}
```

Conceptually, Spring does something like:

```text
Spring IoC Container
        |
        | identifies OrderService
        |
        v
OrderService needs PaymentService
        |
        v
Find/Create PaymentService
        |
        v
Create OrderService
with PaymentService injected
        |
        v
Manage both beans
```

Therefore, Spring has taken control of object creation and dependency wiring.

---

# 2. What is Dependency Injection?

**Dependency Injection (DI)** is a design pattern/mechanism where an object's dependencies are provided to it from the outside instead of the object creating those dependencies itself.

Consider:

```java
class OrderService {

    private final PaymentService paymentService;

    public OrderService(PaymentService paymentService) {
        this.paymentService = paymentService;
    }
}
```

`OrderService` depends on `PaymentService`.

Therefore:

```text
OrderService
     |
     | depends on
     v
PaymentService
```

`PaymentService` is the **dependency**.

The dependency is provided through the constructor.

That is **Dependency Injection**.

---

# 3. IoC vs Dependency Injection

This distinction is important in interviews.

| Concept | Meaning |
|---|---|
| IoC | A broader principle where control over object creation/dependency management is moved outside the application class |
| Dependency Injection | A mechanism/pattern used to implement IoC by supplying dependencies from outside |
| IoC Container | The Spring runtime component that creates, manages, and wires beans |

A useful mental model:

```text
IoC
 |
 | broader principle
 v
Dependency Injection
 |
 | implemented by
 v
Spring IoC Container
```

### Strong interview answer

> **IoC is a broader principle where control over object creation and dependency management is moved away from application code to an external mechanism. Dependency Injection is one of the primary ways Spring implements IoC by supplying an object's dependencies from the Spring IoC container.**

---

# 4. Why do we need Dependency Injection?

Consider:

```java
class OrderService {

    private PaymentService paymentService =
            new PaymentService();
}
```

`OrderService` is tightly coupled to the concrete way `PaymentService` is created.

Suppose we later have:

```text
StripePaymentService
RazorpayPaymentService
MockPaymentService
```

We don't want `OrderService` to be responsible for deciding which implementation to create.

With DI:

```java
class OrderService {

    private final PaymentService paymentService;

    public OrderService(PaymentService paymentService) {
        this.paymentService = paymentService;
    }
}
```

Now `OrderService` does not care how the dependency is created.

It can receive different implementations depending on the configuration.

This promotes:

- Loose coupling
- Easier testing
- Better maintainability
- Better separation of responsibilities
- Easier replacement of implementations

---

# 5. Types of Dependency Injection

There are three commonly discussed types.

## 5.1 Constructor Injection

```java
@Service
class OrderService {

    private final PaymentService paymentService;

    public OrderService(PaymentService paymentService) {
        this.paymentService = paymentService;
    }
}
```

The dependency is provided through the constructor.

### Preferred approach

Constructor injection is generally preferred in modern Spring applications.

---

## 5.2 Setter Injection

```java
@Service
class OrderService {

    private PaymentService paymentService;

    @Autowired
    public void setPaymentService(PaymentService paymentService) {
        this.paymentService = paymentService;
    }
}
```

The dependency is provided through a setter method.

Setter injection can be useful when a dependency is optional or when the dependency needs to be changed after construction.

---

## 5.3 Field Injection

```java
@Service
class OrderService {

    @Autowired
    private PaymentService paymentService;
}
```

Spring injects the dependency directly into the field.

Although this exists and is common in older/existing codebases, constructor injection is generally preferred.

---

# 6. Why is Constructor Injection Preferred?

This is a common interview question.

## 6.1 Mandatory dependencies are explicit

```java
class OrderService {

    private final PaymentService paymentService;

    public OrderService(PaymentService paymentService) {
        this.paymentService = paymentService;
    }
}
```

The constructor clearly communicates:

> `OrderService` cannot be properly constructed without `PaymentService`.

---

## 6.2 Supports immutability

The dependency can be declared as:

```java
private final PaymentService paymentService;
```

It cannot be reassigned after construction.

---

## 6.3 Easier unit testing

You can instantiate the class directly:

```java
PaymentService paymentService = mock(PaymentService.class);

OrderService orderService =
        new OrderService(paymentService);
```

The test does not need the Spring container just to construct the class.

---

## 6.4 Dependencies are visible

Consider:

```java
public OrderService(
        PaymentService paymentService,
        OrderRepository orderRepository,
        NotificationService notificationService) {
}
```

You can immediately see that `OrderService` has three dependencies.

With field injection, these dependencies are less obvious because they are hidden inside the class.

---

# 7. A Simple Real-World Analogy

Imagine a restaurant.

`OrderService` is the waiter.

The waiter needs:

```text
PaymentService
KitchenService
NotificationService
```

The waiter should not build the kitchen or payment system himself.

Instead, those services are provided to him.

Similarly:

```text
Spring Container
      |
      | provides
      v
OrderService
   /     |      \
  v      v       v
Payment Kitchen Notification
Service Service  Service
```

The application class focuses on its business responsibility rather than creating all the objects it needs.

---

# 8. Key Interview Terminology

### Dependency

An object that another object requires to perform its work.

Example:

```text
OrderService -> PaymentService
```

`PaymentService` is a dependency of `OrderService`.

### Injection

The process of providing that dependency from outside the dependent object.

### IoC Container

The Spring component responsible for creating, configuring, wiring, and managing Spring beans.

Common container interfaces:

```java
BeanFactory
ApplicationContext
```

### Bean

An object that is instantiated, configured, and managed by the Spring IoC container.

---

# 9. Common Interview Questions

## Q1. What is IoC?

**Answer:**

> IoC, or Inversion of Control, is a principle where control over object creation and dependency management is moved from application code to an external container or framework. In Spring, the IoC container manages bean creation, dependency resolution, injection, and lifecycle.

---

## Q2. What is Dependency Injection?

**Answer:**

> Dependency Injection is a mechanism where an object's dependencies are provided to it from outside rather than the object creating those dependencies itself. Spring's IoC container resolves and injects these dependencies.

---

## Q3. Are IoC and DI the same?

**Answer:**

> No. IoC is the broader principle of transferring control of object creation and dependency management. Dependency Injection is one of the primary mechanisms used to achieve IoC.

---

## Q4. What are the types of Dependency Injection?

**Answer:**

> Constructor injection, setter injection, and field injection.

Constructor injection is generally preferred for mandatory dependencies.

---

## Q5. Why is constructor injection preferred?

**Answer:**

> Constructor injection makes dependencies explicit, supports immutable fields, ensures required dependencies are supplied during object construction, and makes unit testing easier because the class can be instantiated directly without reflection or a Spring container.

---

## Q6. Who actually creates the dependency in Spring?

The **Spring IoC container** creates and manages the Spring bean and then resolves and injects it into the dependent bean.

Conceptually:

```text
Spring Container
      |
      +---- creates PaymentService
      |
      +---- creates OrderService
                  |
                  +---- receives PaymentService
```

---

# 10. Senior-Level Understanding

A common mistake is to think:

> "Dependency Injection simply means using `@Autowired`."

That is too narrow.

DI is a design principle/pattern.

This is DI even without Spring:

```java
PaymentService paymentService = new PaymentService();

OrderService orderService =
        new OrderService(paymentService);
```

Spring automates and manages this process.

Therefore:

```text
Dependency Injection
        !=
@Autowired
```

`@Autowired` is simply one mechanism Spring can use to identify injection points.

Modern Spring applications commonly use constructor injection without explicitly writing `@Autowired` when there is a single constructor.

Example:

```java
@Service
class OrderService {

    private final PaymentService paymentService;

    public OrderService(PaymentService paymentService) {
        this.paymentService = paymentService;
    }
}
```

Spring can recognize the single constructor and use it for injection.

---

# 11. Final Mental Model

Remember this flow:

```text
                    SPRING
                      |
                      v
              IoC Container
                      |
          +-----------+-----------+
          |                       |
          v                       v
   Create PaymentService   Create OrderService
                                  |
                                  |
                         Needs PaymentService
                                  |
                                  v
                           Inject dependency
                                  |
                                  v
                         Fully initialized
                           OrderService
```

The core idea is:

> **Don't make a class responsible for creating the objects it depends on. Give those dependencies to the class from outside.**

And in Spring:

> **The Spring IoC container takes responsibility for creating, managing, and wiring those objects.**

---

# Interview One-Liner

If the interviewer asks:

**"Explain IoC and DI in one minute."**

A concise answer:

> **IoC is the principle of transferring control of object creation and dependency management from application code to a container. Dependency Injection is a mechanism for achieving IoC, where dependencies are supplied to an object from outside rather than being created by the object itself. In Spring, the IoC container creates and manages beans, resolves their dependencies, and injects them, commonly through constructor injection.**
