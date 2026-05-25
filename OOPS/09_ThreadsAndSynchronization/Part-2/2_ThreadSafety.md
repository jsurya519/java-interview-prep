## Thread Safety and How to Achieve it
Thread Safety means when multiple threads access the same object or piece of code at the same time, the program still behaves correctly, without data corruption or unexpected results.
1. A class or method is thread-safe if it works fine even when accessed by many threads at once.
2. Even if threads run in any order, the shared data will always remain correct.

**What is Atomicity?**
An operation happens completely or not at all, without interruption from other threads. An atomic operation is an operation that behaves like one single indivisible step. All steps happen as one unit.
Example: count++ is not an atomic operation.

## Ways To Achieve Thread Safety in Java:

**1. Using Synchronization**
We use the synchronized keyword to ensure only one thread can access a method or block of code at a time. It helps in preventing race conditions.

```java
class A
{
    synchronized void sum(int n)
    {
        // Creating a thread instance
        Thread t = Thread.currentThread();
        for (int i = 1; i <= 5; i++) {
            System.out.println(t.getName() + " : " + (n + i));
        }
    }
}

// Class B extending thread class
class B extends Thread
{
    // Creating an object of class A
    A a = new A();
    public void run()
    {

        // Calling sum() method
        a.sum(10);
    }
}

class Geeks
{
    public static void main(String[] args)
    {
        // Creating an object of class B
        B b = new B();

        // Initializing instance t1 of Thread class with object of class B
        Thread t1 = new Thread(b);

        // Initializing instance t2 of Thread class with object of class B
        Thread t2 = new Thread(b);

        // Initializing thread t1 with name 'Thread A'
        t1.setName("Thread A");

        // Initializing thread t2 with name 'Thread B'
        t2.setName("Thread B");

        // Starting thread instance t1 and t2
        t1.start();
        t2.start();
    }
}
```

**2. Using Volatile Keyword**
Volatile Keyword ensures visibility of variable changes across threads. It does not guarantee atomicity, but ensures every thread reads the latest value from main memory, not cache.

```java
volatile -> visibility
```
volatile won't work for atomic operations like count++;
Because, count++ internally requires 3 steps

```java
public class Geeks {

    // Initializing volatile variables a, b
    static volatile int a = 0, b = 0;

    // Defining a static void method
    static void method_one() {
        a++;
        b++;
    }

    // Defining static void method
    static void method_two() {
        System.out.println("a=" + a +
                           " b=" + b);
    }

    public static void main(String[] args)
    {
        // Creating an instance t1 of Thread class
        Thread t1 = new Thread()
        {
            public void run()
            {
                for (int i = 0; i < 5; i++)
                    method_one();
            }
        };

        // Creating an instance t2 of Thread class
        Thread t2 = new Thread()
        {
            public void run()
            {
                for (int i = 0; i < 5; i++)
                    method_two();
            }
        };

        // Starting instance t1 and t2
        t1.start();
        t2.start();
    }
}
```
The volatile keyword ensures that variables are always read from main memory, so changes made by one thread are immediately visible to others. As a result, when one thread updates the values, other threads see the updated values instantly.

**3. Using Atomic Variable:**
Using an atomic variable is another way to achieve thread-safety in java. When variables are shared by multiple threads, the atomic variable ensures that threads don't crash into each other.

```java
import java.util.concurrent.atomic.AtomicInteger;

class Counter
{
    // Creating a variable of class type AtomicInteger
    AtomicInteger count
        = new AtomicInteger();

    // Defining increment() method to change value of AtomicInteger variable
    public void increment() {
        count.incrementAndGet();
    }
}

public class TestCounter {
    public static void main(String[] args)
      		throws Exception
    {
        // Creating an instance of Counter class
        Counter c = new Counter();

        // Creating an instance t1 of Thread class
        Thread t1 = new Thread(new Runnable()
        {
        	public void run() {
            	for(int i = 1; i <= 2000; i++) {
                	c.increment();
                }
            }
        });

        // Creating an instance t2 of Thread class
        Thread t2 = new Thread(new Runnable()
        {
        	public void run() {
              	for (int i = 1; i <= 2000; i++) {
                  	c.increment();
                }
            }
        });

        // Calling start() method with t1 and t2
        t1.start();
        t2.start();

        // Calling join method with t1 and t2
        t1.join();
        t2.join();

        System.out.println(c.count);
    }
}
// Output:
// 4000
```
AtomicInteger uses compare-and-swap (CAS) to perform operations atomically without explicit synchronization. As a result, incrementAndGet() safely updates the shared counter, allowing multiple threads to modify it without conflicts.

**4. Using Immutable Objects:**
Immutable objects are inherently thread-safe, as their state cannot change after creation. Mark fields as final and don’t provide setters.

```java
final class Student {
    private final String name;
    private final int rollNo;

    public Student(String name, int rollNo) {
        this.name = name;
        this.rollNo = rollNo;
    }

    public String getName() {
        return name;
    }

    public int getRollNo() {
        return rollNo;
    }
}
```

## Thread Safety vs Non-thread Safety
# Thread-Safe vs Non-Thread Safe

| Feature | Thread-Safe | Non-Thread Safe |
|---|---|---|
| Definition | Used to handle concurrent access by multiple threads safely | Not designed for safe concurrent access by multiple threads |
| Synchronization | Uses synchronization internally for thread safety | Does not use synchronization; access must be manually managed |
| Use Case | Used in multi-threaded environments | Mainly used for single-threaded scenarios or with external synchronization |
| Performance | Slower due to synchronization overhead | Faster, as there’s no locking mechanism |
| Scalability | May not scale well under high concurrency due to lock contention | Scales well if used in a controlled single-threaded context |
| Examples | Vector, Hashtable, ConcurrentHashMap, StringBuffer | ArrayList, HashMap, StringBuilder, SimpleDateFormat |