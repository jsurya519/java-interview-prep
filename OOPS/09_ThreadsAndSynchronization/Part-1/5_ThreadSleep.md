## Thread sleep:
Thread.sleep() used to pause the execution of the currently running thread for a specified amount of time. After the sleep duration ends, the thread becomes runnable again and continues execution based on thread scheduling.

1. Throws InterruptedException if another thread interrupts during sleep.
2. Actual sleep duration may vary based on system load; higher load increases sleep time.

```java
public class SleepDemo {
    public static void main(String[] args) throws InterruptedException {
        for (int i = 0; i < 3; i++) {
            System.out.print(i + " ");
            Thread.sleep(1000*5);
        }
    }
}
```

## Variations of sleep()
There are 2 variations of the sleep() method in Java Thread. These are:
```java
public static void sleep(long millis)
public static void sleep(long millis, int nanos)
```

**millis:**  Duration of time in milliseconds for which thread will sleep
**nanos:** This is the additional time in nanoseconds for which we want the thread to sleep. It ranges from 0 to 999999..

```java
import java.lang.Thread;

// Class extending the Thread Class
class MyThread extends Thread
{
    // Overriding the run method
    @Override
    public void run()
    {
        // use throws keyword followed by exception
      	// name for throwing the exception
        try {
            for (int i = 0; i < 5; i++) {

                // method will sleep the thread
                Thread.sleep(1000);

                System.out.print(i+" ");
            }
        }
        catch (Exception e) {
            // catching the exception
            System.out.println(e);
        }
    }

    public static void main(String[] args)
    {
        // created thread
        MyThread obj = new MyThread();
        obj.start();
    }
}
```

**IllegalArgumentException when sleep time is Negative**

```java
import java.lang.Thread;

class Geeks
{
    public static void main(String[] args)
    {
        // Use throws keyword followed by exception
      	// name for throwing the exception
        try {
            for (int i = 0; i < 5; i++) {

                // this will throw the
                // IllegalArgumentException
                Thread.sleep(-100);

                // Printing the value of the variable
                System.out.println(i);
            }
        }
        catch (Exception e) {
            // Catching the exception
            System.out.println(e);
        }
    }
}
// Output:
// java.lang.IllegalArgumentException: timeout value is negative
```

