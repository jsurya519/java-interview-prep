## Synchronization:
Synchronization is used to control the execution of multiple processes or threads so that shared resources are accessed in a proper and orderly manner. It helps avoid conflicts and ensures correct results when many tasks run at the same time.
1. t controls the access of shared resources.
2. It avoids data inconsistency.
3. It ensures proper execution of processes.

**Critical Section:**
A critical section is a part of code where shared resources/data are accessed or modified by multiple threads.
Since multiple threads can execute simultaneously, only one thread should execute the critical section at a time to avoid inconsistent data.

**race condition:**
A race condition happens when multiple threads access and modify shared data simultaneously, and the final result depends on which thread executes first.

## Ways to Achieve Synchronization:
<img width="800" height="288" alt="image" src="https://github.com/user-attachments/assets/95e993ec-74f3-4811-865f-51ef6419d935" />


**1. Synchronized Methods:**
Synchronized methods are used to lock an entire method so that only one thread can execute it at a time for a particular object. This ensures safe access to shared data but may reduce performance due to full method locking.
1. Locks the whole method, not just a part of it.
2. Uses the object-level lock (instance lock).

```java
class Counter{

    // Shared variable
    private int c = 0;

    // Synchronized method to increment counter
    public synchronized void inc(){
        c++; // critical section;

    }

    // Synchronized method to get counter value
    public synchronized int get(){
        return c;

    }
}

public class Geeks{

    public static void main(String[] args){

        // Shared resource
        Counter cnt = new Counter();

        Thread t1 = new Thread(() -> {
            for (int i = 0; i < 1000; i++)
                cnt.inc();
        });

        Thread t2 = new Thread(() -> {
            for (int i = 0; i < 1000; i++)
                cnt.inc();
        });

        t1.start();
        t2.start();

        try {
            t1.join();
            t2.join();
        }
        catch (InterruptedException e) {
            e.printStackTrace();
        }

        System.out.println("Counter: " + cnt.get());
    }
}
// Output:
// Counter: 2000
```


**2. Synchronized Blocks:**
Synchronized blocks allow locking only a specific section of code instead of the entire method. This makes the program more efficient by reducing the scope of synchronization.
1. Locks only the **critical section** of code, not the entire method.
2. Provides better performance due to fine-grained control.

```java
class Counter{

    private int c = 0;

    public void inc(){

        // Synchronize only this block
        synchronized (this) { c++; }
    }

    public int get() { return c; }
}

public class Geeks {

    public static void main(String[] args)
        throws InterruptedException{

        Counter cnt = new Counter();

        Thread t1 = new Thread(() -> {
            for (int i = 0; i < 1000; i++)
                cnt.inc();
        });

        Thread t2 = new Thread(() -> {
            for (int i = 0; i < 1000; i++)
                cnt.inc();
        });

        t1.start();
        t2.start();
        t1.join();
        t2.join();

        System.out.println("Counter: " + cnt.get());
    }
}
// Output:
// Counter: 2000
```


**3. Static Synchronization:**
Static synchronization is used when static data or methods need to be protected in a multithreaded environment. It ensures that only one thread can access the class-level resource at a time.
1. Locks at the class level instead of the object level.
2. Shared across all instances of the class.

```java
class Table{

    synchronized static void printTable(int n){

        for (int i = 1; i <=3; i++){

            System.out.println(n * i);
            try {
            } catch (Exception e) {
                System.out.println(e);
            }
        }
    }
}

class Thread1 extends Thread{

    public void run() {
        Table.printTable(1);
    }
}

class Thread2 extends Thread {
    public void run() {
        Table.printTable(10);
    }
}

public class GFG{

    public static void main(String[] args){

        Thread1 t1 = new Thread1();
        Thread2 t2 = new Thread2();
        t1.start();
        t2.start();
    }
}
// Output:
// 1
// 2
// 3
// 10
// 20
// 30
```


```java
class Counter {
    static int c=0;

    static void increment()
    {
        synchronized (Counter.class)
        {
            for(int i=0;i<10;i++)
                c++;
        }
    }
}

public class Dummy {

    public static void main(String[] args) throws InterruptedException {

        Thread t1 = new Thread(()->{
            Counter.increment();
        });

        Thread t2 = new Thread(()->{
            Counter.increment();
        });

        t1.start();
        t2.start();

        t1.join();
        t2.join();

        System.out.println(Counter.c);

    }
}
// Output:
// 20
```

**Note:**
With synchronized method or block, Java automatically acquires the lock associated with an object. That internal lock mechanism is called the monitor/intrinsic lock (or simply monitor).
```java
synchronized(this) {
    count++;
}
```

Here:
1. thread acquires monitor lock of this object
2. only one thread can hold that monitor at a time
3. other threads wait
