## Main Thread:
When a Java program starts, the Java Virtual Machine (JVM) creates a thread automatically called the main thread. This thread executes the main() method and controls the overall execution flow of the program.
1. It is the parent thread from which all other user-defined threads are created.
2. The default name of the main thread is "main".
3. The default priority of the main thread is 5.
4. It usually finishes last as it may perform cleanup and shutdown tasks.

<img width="690" height="400" alt="image" src="https://github.com/user-attachments/assets/0f1840ad-6a2b-48d5-bec6-2c71b5ed99d9" />


```java
import static java.lang.Thread.*;

public class Dummy {

    public static void main(String[] args){

            // Getting reference to Main thread
            Thread t = Thread.currentThread();

            // Getting name of Main thread
            System.out.println("Current thread: "
                    + t.getName());

            // Changing the name of Main thread
            t.setName("Geeks");
            System.out.println("After name change: "
                    + t.getName());

            // Getting priority of Main thread
            System.out.println("Main thread priority: "
                    + t.getPriority());

            // Setting priority of Main thread to MAX(10)
            t.setPriority(MAX_PRIORITY);

            // Print and display the main thread priority
            System.out.println("Main thread new priority: "
                    + t.getPriority());

            for (int i = 0; i < 5; i++) {
                System.out.println("Main thread");
            }

            // Main thread creating a child thread
            Thread ct = new Thread() {
                // run() method of a thread
                public void run()
                {

                    for (int i = 0; i < 5; i++) {
                        System.out.println("Child thread");
                    }
                }
            };

            // Getting priority of child thread
            // which will be inherited from Main thread
            // as it is created by Main thread
            System.out.println("Child thread priority: "
                    + ct.getPriority());

            // Setting priority of Main thread to MIN(1)
            ct.setPriority(MIN_PRIORITY);

            System.out.println("Child thread new priority: "
                    + ct.getPriority());

            // Starting child thread
            ct.start();
        }
}
// Output:
// Current thread: main
// After name change: Geeks
// Main thread priority: 5
// Main thread new priority: 10
// Main thread
// Main thread
// Main thread
// Main thread
// Main thread
// Child thread priority: 10
// Child thread new priority: 1
// Child thread
// Child thread
// Child thread
// Child thread
// Child thread
```

**Explanation:**
1. Thread.currentThread() returns the main thread.
2. getName() and setName() allow reading and changing the thread name.
3. getPriority() and setPriority() allow reading and changing the thread priority.
4. The main thread can create child threads, which by default inherit the main thread’s priority.

## Relationship Between main() Method and Main Thread
1. For every Java program, JVM creates the main thread first.
2. The main thread checks for the existence of the main() method, then initializes the class.
3. Since JDK 6, the main() method is mandatory for a standalone Java application.

---

## Understanding join method:
join() is used when one thread must wait until another thread finishes execution.

**Example without join:**
```java
class MyThread extends Thread {
    public void run() {

        for(int i = 1; i <= 5; i++) {
            System.out.println("Child Thread");
        }
    }
}

public class Test {

    public static void main(String[] args) {

        MyThread t = new MyThread();
        t.start();

        for(int i = 1; i <= 5; i++) {
            System.out.println("Main Thread");
        }
    }
}
// Output:
// Main Thread
// Child Thread
// Main Thread
// Child Thread
// ...
```
Output order is unpredictable.

**Example With join()**

```java
class MyThread extends Thread {

    public void run() {

        for(int i = 1; i <= 5; i++) {
            System.out.println("Child Thread");
        }
    }
}

public class Test {

    public static void main(String[] args) throws Exception {

        MyThread t = new MyThread();

        t.start();

        t.join();

        for(int i = 1; i <= 5; i++) {
            System.out.println("Main Thread");
        }
    }
}
// Output:
// Child Thread
// Child Thread
// Child Thread
// Child Thread
// Child Thread
// Main Thread
// Main Thread
// ...
```


When t.join is  executed,
1. Current thread goes into WAITING state
2. It waits until thread t dies/completes
3. After t finishes, waiting thread resumes

## Types of join:

**1. Wait until completion**
```java
t.join();
```

**2. Wait maximum given milliseconds**
```java
t.join(5000);
```

**3. Milliseconds + nanoseconds**
```java
t.join(2000, 500000);
```

---

## Deadlock Using Main Thread:

```java
public class GFG {

  public static void main(String[] args) {

    // Try block to check for exceptions
    try {

      // Print statement
      System.out.println("Entering into Deadlock");

      // Joining the current thread
      Thread.currentThread().join();

      // This statement will never execute
      System.out.println("This statement will never execute");
    }

    // Catch block to handle the exceptions
    catch (InterruptedException e) {

      // Display the exception along with line number
      // using printStackTrace() method
      e.printStackTrace();
    }
  }
}
```

Output:

<img width="573" height="155" alt="image" src="https://github.com/user-attachments/assets/d4875f2c-e7fe-4135-94a2-c3c6c3832ff9" />


**Explanation: **
1. Thread.currentThread().join() tells the main thread to wait for itself to die.
2. The thread waits indefinitely, causing a deadlock.
3. Any code after join() will never execute.
