# Spring Boot Interview Preparation

# 2. Spring Bean Lifecycle

## 2.1 What is the Spring Bean Lifecycle?

The **Spring Bean Lifecycle** describes the sequence of steps Spring follows from the time it creates a bean until the time it destroys that bean.

At a high level:

```text
Bean Definition
      ↓
Bean Instantiation
      ↓
Dependency Injection
      ↓
Aware callbacks
      ↓
BeanPostProcessor - Before Initialization
      ↓
Initialization callbacks
      ↓
BeanPostProcessor - After Initialization
      ↓
Bean is Ready
      ↓
Bean is Used
      ↓
Bean Destruction
```

The important idea is:

> Spring doesn't simply call `new` and return the object. The container performs several steps to create, configure, initialize, post-process, and eventually destroy the bean.

---

# 2.2 Simple Example

Consider:

```java
@Component
public class PaymentService {

    private String name;

    public PaymentService() {
        System.out.println("1. Constructor");
    }

    @Autowired
    public void setName(SomeDependency dependency) {
        System.out.println("2. Dependency injection");
    }

    @PostConstruct
    public void init() {
        System.out.println("3. @PostConstruct");
    }

    @PreDestroy
    public void destroy() {
        System.out.println("4. @PreDestroy");
    }
}
```

The important lifecycle sequence is approximately:

```text
Constructor
    ↓
Dependency Injection
    ↓
@PostConstruct
    ↓
Bean is ready
    ↓
@PreDestroy
```

But for interviews, we need to understand the complete lifecycle and the role of `BeanPostProcessor`.

---

# 2.3 Complete Bean Lifecycle

A simplified but interview-useful sequence is:

```text
1. Spring reads BeanDefinition
             ↓
2. Bean instance is created
             ↓
3. Dependencies are injected
             ↓
4. Aware callbacks
             ↓
5. BeanPostProcessor.postProcessBeforeInitialization()
             ↓
6. @PostConstruct
             ↓
7. InitializingBean.afterPropertiesSet()
             ↓
8. Custom init-method
             ↓
9. BeanPostProcessor.postProcessAfterInitialization()
             ↓
10. Bean is ready for use
             ↓
11. Application runs
             ↓
12. Container shuts down
             ↓
13. @PreDestroy
             ↓
14. DisposableBean.destroy()
             ↓
15. Custom destroy-method
```

This is the sequence worth understanding for interviews.

---

# 2.4 Step 1 — Bean Definition

Before Spring creates a bean, it needs to know about the bean.

For example:

```java
@Component
public class PaymentService {
}
```

Spring's component scanning detects this class and creates metadata describing the bean.

That metadata is represented internally through a **BeanDefinition**.

A `BeanDefinition` contains information such as:

- Bean class
- Scope
- Dependencies
- Initialization method
- Destruction method
- Lazy initialization
- Autowiring information

Conceptually:

```text
PaymentService.class
       ↓
BeanDefinition
       ↓
Spring Container knows how to create/manage it
```

---

# 2.5 Step 2 — Bean Instantiation

Spring creates the bean instance.

Conceptually:

```java
PaymentService paymentService =
        new PaymentService();
```

The constructor executes here.

For example:

```java
@Component
public class PaymentService {

    public PaymentService() {
        System.out.println("Constructor");
    }
}
```

Output:

```text
Constructor
```

Important:

> Constructor execution happens before dependency injection.

---

# 2.6 Step 3 — Dependency Injection

After the object is instantiated, Spring populates its dependencies.

Example:

```java
@Component
public class OrderService {

    private PaymentService paymentService;

    @Autowired
    public void setPaymentService(PaymentService paymentService) {
        this.paymentService = paymentService;
    }
}
```

Conceptually:

```text
new OrderService()
       ↓
PaymentService is resolved
       ↓
PaymentService is injected
```

For constructor injection, the dependency is resolved before the constructor is invoked because Spring needs the constructor arguments to create the object.

Therefore, be careful with the simplified statement "constructor → dependency injection":

- With **setter/field injection**, the object is instantiated first and dependencies are populated afterward.
- With **constructor injection**, dependencies are resolved and supplied as part of construction.

This distinction is useful in senior-level interviews.

---

# 2.7 Step 4 — Aware Interfaces

Spring can provide certain container-related information to a bean through `Aware` interfaces.

Common examples:

```java
BeanNameAware
BeanFactoryAware
ApplicationContextAware
```

Example:

```java
@Component
public class MyBean implements BeanNameAware {

    @Override
    public void setBeanName(String name) {
        System.out.println("Bean name = " + name);
    }
}
```

Spring calls:

```java
setBeanName(...)
```

This allows the bean to become aware of information about its container-managed environment.

### Important interview point

Do not say that every bean must implement `Aware`.

They are optional callback interfaces.

Also, application code generally should not depend heavily on these interfaces because doing so couples the class to Spring.

---

# 2.8 Step 5 — BeanPostProcessor Before Initialization

This is one of the most important Spring concepts.

Spring invokes:

```java
postProcessBeforeInitialization()
```

from registered `BeanPostProcessor`s.

Conceptually:

```java
bean = postProcessBeforeInitialization(bean, beanName);
```

This happens before the normal initialization callbacks.

This mechanism allows Spring and applications to modify or wrap beans during their lifecycle.

---

# 2.9 Step 6 — @PostConstruct

After the "before initialization" post-processing, Spring invokes initialization callbacks.

One commonly used callback is:

```java
@PostConstruct
public void init() {
    // initialization logic
}
```

Example:

```java
@Component
public class PaymentService {

    @PostConstruct
    public void init() {
        System.out.println("Bean initialized");
    }
}
```

Typical use cases:

- Validate configuration
- Initialize internal state
- Prepare resources
- Perform startup logic that depends on injected dependencies

Important:

> `@PostConstruct` runs after dependency injection, so injected dependencies are available.

---

# 2.10 Step 7 — InitializingBean.afterPropertiesSet()

A bean can implement:

```java
InitializingBean
```

and override:

```java
afterPropertiesSet()
```

Example:

```java
@Component
public class PaymentService implements InitializingBean {

    @Override
    public void afterPropertiesSet() {
        System.out.println("afterPropertiesSet");
    }
}
```

Spring invokes this callback after the bean's properties have been populated.

### When does it run?

The simplified order is:

```text
@PostConstruct
      ↓
afterPropertiesSet()
```

---

# 2.11 Step 8 — Custom init-method

You can define a custom initialization method.

For example:

```java
@Configuration
public class AppConfig {

    @Bean(initMethod = "init")
    public PaymentService paymentService() {
        return new PaymentService();
    }
}
```

And:

```java
public class PaymentService {

    public void init() {
        System.out.println("Custom init method");
    }
}
```

The simplified initialization sequence is:

```text
@PostConstruct
      ↓
InitializingBean.afterPropertiesSet()
      ↓
Custom init-method
```

---

# 2.12 Step 9 — BeanPostProcessor After Initialization

After initialization callbacks, Spring invokes:

```java
postProcessAfterInitialization()
```

from the registered `BeanPostProcessor`s.

Conceptually:

```text
Bean
 ↓
postProcessBeforeInitialization()
 ↓
Initialization callbacks
 ↓
postProcessAfterInitialization()
 ↓
Ready bean
```

This stage is particularly important because Spring can return a **different object** from the original bean.

For example, Spring may create a proxy around a bean.

---

# 2.13 Why is BeanPostProcessor so important?

A `BeanPostProcessor` can inspect or modify beans before and after initialization.

Spring itself uses this mechanism extensively.

It is involved in many framework features, including processing annotations and creating proxies.

A simplified example:

```java
@Component
public class MyBeanPostProcessor
        implements BeanPostProcessor {

    @Override
    public Object postProcessBeforeInitialization(
            Object bean,
            String beanName) {

        System.out.println(
                "Before initialization: " + beanName);

        return bean;
    }

    @Override
    public Object postProcessAfterInitialization(
            Object bean,
            String beanName) {

        System.out.println(
                "After initialization: " + beanName);

        return bean;
    }
}
```

This gives us an important interview insight:

> `BeanPostProcessor` operates on beans during their lifecycle and can even return a wrapped/proxied object.

---

# 2.14 Step 10 — Bean Is Ready

After:

```text
postProcessAfterInitialization()
```

the bean is considered fully initialized and available for use by the application.

Conceptually:

```text
Created
   ↓
Configured
   ↓
Initialized
   ↓
Post-processed
   ↓
READY
```

---

# 2.15 Step 11 — Bean Is Used

The application now uses the bean.

For example:

```java
orderService.createOrder();
```

The bean remains managed by the Spring container according to its scope.

For a typical singleton bean:

```text
One bean instance
       |
       +---- used by many components
```

---

# 2.16 Step 12 — Container Shutdown

When the Spring ApplicationContext shuts down, Spring begins destroying beans that it manages.

For example:

```java
context.close();
```

or during normal application shutdown.

This starts the destruction lifecycle.

---

# 2.17 Step 13 — @PreDestroy

Spring invokes:

```java
@PreDestroy
public void cleanup() {
    // cleanup logic
}
```

Example:

```java
@Component
public class PaymentService {

    @PreDestroy
    public void cleanup() {
        System.out.println("Cleaning up");
    }
}
```

Typical use cases:

- Release resources
- Close connections
- Stop background tasks
- Cleanup internal state

---

# 2.18 Step 14 — DisposableBean.destroy()

A bean can implement:

```java
DisposableBean
```

and override:

```java
destroy()
```

Example:

```java
@Component
public class PaymentService implements DisposableBean {

    @Override
    public void destroy() {
        System.out.println("Destroying bean");
    }
}
```

The simplified destruction sequence is:

```text
@PreDestroy
     ↓
DisposableBean.destroy()
     ↓
Custom destroy-method
```

---

# 2.19 Step 15 — Custom destroy-method

You can define a custom destroy method:

```java
@Bean(destroyMethod = "cleanup")
public PaymentService paymentService() {
    return new PaymentService();
}
```

Then:

```java
public class PaymentService {

    public void cleanup() {
        System.out.println("Cleanup");
    }
}
```

This is invoked during bean destruction.

---

# 2.20 Complete Lifecycle Diagram

Keep this diagram in your interview notes:

```text
                 Bean Definition
                       |
                       v
                 Bean Creation
                       |
                       v
              Dependency Injection
                       |
                       v
                 Aware Callbacks
                       |
                       v
        BeanPostProcessor - BEFORE
             Initialization
                       |
                       v
                 @PostConstruct
                       |
                       v
        InitializingBean.afterPropertiesSet()
                       |
                       v
               Custom init-method
                       |
                       v
        BeanPostProcessor - AFTER
             Initialization
                       |
                       v
                  BEAN READY
                       |
                       v
                  Bean in use
                       |
                       v
              Application Shutdown
                       |
                       v
                  @PreDestroy
                       |
                       v
             DisposableBean.destroy()
                       |
                       v
              Custom destroy-method
                       |
                       v
                Bean Destroyed
```

---

# 2.21 Important Callback Ordering

For interviews, remember these two sequences.

### Initialization

```text
@PostConstruct
     ↓
InitializingBean.afterPropertiesSet()
     ↓
Custom init-method
```

with `BeanPostProcessor` surrounding initialization:

```text
postProcessBeforeInitialization()
     ↓
@PostConstruct
     ↓
afterPropertiesSet()
     ↓
custom init-method
     ↓
postProcessAfterInitialization()
```

### Destruction

```text
@PreDestroy
     ↓
DisposableBean.destroy()
     ↓
Custom destroy-method
```

---

# 2.22 Singleton vs Prototype and Destruction

This is a very important interview follow-up.

Spring manages the complete lifecycle of **singleton beans** by default.

For example:

```java
@Component
public class PaymentService {
}
```

is singleton-scoped by default.

Spring creates and manages its lifecycle and destruction.

However, **prototype-scoped beans are different**.

```java
@Scope("prototype")
@Component
public class PaymentService {
}
```

For prototype beans:

- Spring creates the bean when requested.
- Spring performs initialization callbacks.
- After handing the bean to the caller, Spring generally does **not** manage its complete destruction lifecycle.

Therefore, destruction callbacks such as `@PreDestroy` are not automatically invoked for prototype beans by the Spring container.

### Interview answer

> Spring manages the complete lifecycle of singleton beans, including destruction. For prototype-scoped beans, Spring creates and initializes them but does not generally manage their destruction after handing them to the caller.

---

# 2.23 Does Spring create all singleton beans at startup?

Not necessarily.

By default, singleton beans are eagerly created during ApplicationContext startup.

But lazy initialization can change this behavior.

Example:

```java
@Lazy
@Component
public class PaymentService {
}
```

With lazy initialization, the bean is created when it is first needed rather than during startup.

So the lifecycle begins when the bean is actually instantiated.

---

# 2.24 Common Interview Questions

## Q1. What is the Spring Bean Lifecycle?

**Answer:**

> The Spring Bean Lifecycle is the sequence through which the Spring IoC container creates, configures, initializes, manages, and eventually destroys a bean. It includes instantiation, dependency injection, aware callbacks, BeanPostProcessor callbacks, initialization callbacks such as `@PostConstruct`, and destruction callbacks such as `@PreDestroy`.

---

## Q2. What is the order of @PostConstruct, afterPropertiesSet(), and custom init-method?

**Answer:**

```text
@PostConstruct
      ↓
afterPropertiesSet()
      ↓
custom init-method
```

---

## Q3. Where does BeanPostProcessor fit into the lifecycle?

**Answer:**

```text
postProcessBeforeInitialization()
        ↓
initialization callbacks
        ↓
postProcessAfterInitialization()
```

It allows Spring or application code to inspect, modify, wrap, or replace bean instances during lifecycle processing.

---

## Q4. What is the difference between @PostConstruct and afterPropertiesSet()?

Both are initialization callbacks.

### @PostConstruct

Standard annotation-based callback:

```java
@PostConstruct
public void init() {
}
```

### afterPropertiesSet()

Spring-specific interface:

```java
public class MyBean implements InitializingBean {

    @Override
    public void afterPropertiesSet() {
    }
}
```

Generally, `@PostConstruct` is preferred because it avoids coupling the class directly to the Spring-specific `InitializingBean` interface.

---

## Q5. What is the difference between @PreDestroy and DisposableBean?

Same idea as initialization callbacks.

### @PreDestroy

Annotation-based:

```java
@PreDestroy
public void cleanup() {
}
```

### DisposableBean

Spring-specific interface:

```java
public class MyBean implements DisposableBean {

    @Override
    public void destroy() {
    }
}
```

`@PreDestroy` is generally preferred because it avoids direct coupling to Spring's lifecycle interface.

---

## Q6. Can BeanPostProcessor change the bean?

Yes.

A `BeanPostProcessor` can return the same bean:

```java
return bean;
```

or a different object:

```java
return proxy;
```

This is important because Spring can use this mechanism to wrap beans with proxies.

---

## Q7. When does the constructor execute relative to dependency injection?

It depends on the injection mechanism.

### Constructor injection

Dependencies are resolved and supplied as constructor arguments before/while the object is constructed.

```java
public OrderService(PaymentService paymentService) {
}
```

### Field/setter injection

The object is instantiated first, and dependencies are populated afterward.

```text
Constructor
    ↓
Field/Setter injection
```

This is one reason constructor injection is useful: required dependencies are available as part of object construction.

---

## Q8. Does @PostConstruct run before or after dependency injection?

For field/setter injection:

```text
Bean instantiated
      ↓
Dependencies injected
      ↓
@PostConstruct
```

Therefore, dependencies should be available inside `@PostConstruct`.

---

## Q9. Does Spring destroy prototype beans?

Generally, no.

Spring creates and initializes prototype beans but does not manage their complete destruction lifecycle after returning them to the caller.

---

# 2.24 Senior-Level Interview Trap

### Question:

> "Is the object returned by `postProcessAfterInitialization()` always the same object that was instantiated?"

**Answer:**

No.

A `BeanPostProcessor` can return the original bean:

```java
return bean;
```

or another object, such as a proxy:

```java
return proxy;
```

Therefore:

```text
Original Bean
     ↓
BeanPostProcessor
     ↓
Proxy / Wrapped Bean
```

This is one of the mechanisms behind Spring's extensive use of proxies.

---

# 2.25 Another Senior-Level Question

### Question:

> "Why does Spring need BeanPostProcessor?"

Because Spring needs a general extension point to process beans during their lifecycle.

It allows the framework to:

- Inspect beans
- Modify beans
- Apply additional behavior
- Wrap beans
- Create proxies
- Process annotations and framework features

Conceptually:

```text
Bean
 ↓
Before Processing
 ↓
Initialization
 ↓
After Processing
 ↓
Possibly proxied bean
```

---

# 2.26 Important Mental Model

Do not memorize the lifecycle as a random list.

Think in four phases:

```text
1. CREATE
   ↓
2. CONFIGURE
   ↓
3. INITIALIZE
   ↓
4. DESTROY
```

### CREATE

```text
BeanDefinition
    ↓
Instantiation
```

### CONFIGURE

```text
Dependency Injection
    ↓
Aware callbacks
```

### INITIALIZE

```text
BeanPostProcessor BEFORE
    ↓
@PostConstruct
    ↓
afterPropertiesSet()
    ↓
custom init-method
    ↓
BeanPostProcessor AFTER
```

### DESTROY

```text
@PreDestroy
    ↓
DisposableBean.destroy()
    ↓
custom destroy-method
```

This mental model is much easier to remember during an interview.

---

# 2.27 One-Minute Interview Answer

If the interviewer asks:

**"Explain the Spring Bean Lifecycle."**

A strong answer:

> "Spring first creates a BeanDefinition and uses it to instantiate the bean. For constructor injection, required dependencies are resolved as part of construction; for field or setter injection, dependencies are populated after instantiation. Spring then invokes applicable Aware callbacks and BeanPostProcessor's `postProcessBeforeInitialization`. Initialization callbacks such as `@PostConstruct`, `InitializingBean.afterPropertiesSet()`, and a configured custom init method are then executed. After that, `postProcessAfterInitialization` is called, and the bean is ready for use. During container shutdown, destruction callbacks such as `@PreDestroy`, `DisposableBean.destroy()`, and a custom destroy method are invoked for beans whose destruction is managed by the container."

---

# 2.28 Quick Revision

```text
CREATE
    BeanDefinition
        ↓
    Instantiate

CONFIGURE
        ↓
    Dependency Injection
        ↓
    Aware callbacks

INITIALIZE
        ↓
    postProcessBeforeInitialization()
        ↓
    @PostConstruct
        ↓
    afterPropertiesSet()
        ↓
    custom init-method
        ↓
    postProcessAfterInitialization()

READY
        ↓
    Bean is used

DESTROY
        ↓
    @PreDestroy
        ↓
    DisposableBean.destroy()
        ↓
    custom destroy-method
```

## Key things to remember

1. **BeanPostProcessor is central to Spring bean lifecycle processing.**
2. `@PostConstruct` runs after dependencies have been populated.
3. Initialization callback order:

```text
@PostConstruct
→ afterPropertiesSet()
→ custom init-method
```

4. Destruction callback order:

```text
@PreDestroy
→ destroy()
→ custom destroy-method
```

5. Constructor injection is different from field/setter injection in when dependencies are supplied.
6. Singleton beans have container-managed destruction; prototype beans generally do not.
7. `BeanPostProcessor` can return a proxy/different object.
