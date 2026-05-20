## Daemon Thread:
A daemon thread is a background thread in Java that supports user threads and does not prevent the JVM from exiting when all user threads finish. It is ideal for background tasks like monitoring, logging, and cleanup.

1. Runs in the background to support user (non-daemon) threads.
2. JVM exits automatically when all user threads finish.
3. Created using the Thread class and marked as daemon with setDaemon(true).
4. setDaemon(true) must be called before starting the thread, or it throws IllegalThreadStateException.
5. Common examples: Garbage Collector (GC) and Finalizer Thread.

```java
class MyDaemonThread extends Thread {
    public void run() {
        System.out.println(getName() + " is running as a daemon thread.");
    }
}

public class GFG {
    public static void main(String[] args) throws InterruptedException {
        MyDaemonThread t1 = new MyDaemonThread();
        t1.setDaemon(true);  // mark as daemon
        t1.setName("Daemon-1");
        t1.start();

        // Give JVM a moment to run daemon thread
        Thread.sleep(1000*10);

        System.out.println("Main thread ends.");
    }
}
// Output:
// Daemon-1 is running as a daemon thread.
// Main thread ends.
```

Once the main thread ends, the JVM terminates all daemon threads automatically.

## Methods Used:
**void setDaemon(boolean on):** Marks a thread as daemon or user thread. Must be called before start().
**boolean isDaemon():** Checks whether a thread is daemon.

```java
public class DaemonExample extends Thread {
    public void run() {
        if (Thread.currentThread().isDaemon()) {
            System.out.println("Daemon thread running...");
        } else {
            System.out.println("User thread running...");
        }
    }

    public static void main(String[] args) {
        DaemonExample t1 = new DaemonExample();
        DaemonExample t2 = new DaemonExample();

        t1.setDaemon(true);  // must be set before start()

        t1.start();
        t2.start();
    }
}
// Output:
// Daemon thread running...
// User thread running...
```

## Behavior of Daemon Thread:
```java
public class DaemonBehavior extends Thread {
    public void run() {
        while (true) {
            System.out.println("Daemon thread running...");
        }
    }

    public static void main(String[] args) {
        DaemonBehavior t = new DaemonBehavior();
        t.setDaemon(true);
        t.start();

        System.out.println("Main (user) thread ends...");
    }
}
// Output:
// Main (user) thread ends...
// Daemon thread running...
// Daemon thread running...
// Daemon thread running...
// Daemon thread running...
```

The JVM ends immediately after the main thread finishes, even though the daemon thread is still running.

**Notes:**
1. A thread inherits the daemon status of the thread that creates it.
2. Daemon threads should not be used for tasks requiring completion, such as writing to a file or updating a database.
3. JVM terminates all daemon threads abruptly without performing cleanup operations.
