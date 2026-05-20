## Thread Priority:
Java supports multithreading, where multiple threads run concurrently and the Thread Scheduler decides their execution order. Each thread is assigned a priority (1–10) that influences scheduling but does not guarantee execution order.

1. Changing priority does not guarantee faster execution or immediate scheduling
2. Higher-priority threads are generally preferred by the scheduler
3. Actual execution order depends on the JVM and underlying OS

```java
class SimpleThread extends Thread {
    public void run() {
        System.out.println(getName() + " is running with priority " + getPriority());
    }
}

public class GFG {
    public static void main(String[] args) {
        SimpleThread t1 = new SimpleThread();
        SimpleThread t2 = new SimpleThread();

        t1.setName("HighPriorityThread");
        t2.setName("LowPriorityThread");

        t1.setPriority(Thread.MAX_PRIORITY); // 10
        t2.setPriority(Thread.MIN_PRIORITY); // 1

        t1.start();
        t2.start();
    }
}
// Output:
// LowPriorityThread is running with priority 1
// HighPriorityThread is running with priority 10
```

Higher-priority threads may get more CPU time, but the actual execution order is not guaranteed because it depends on the JVM and the OS scheduler.

![0e16fa7010966ba3c5d9eb05e6384c27.png](:/3431859617904d7d8df3c0ac5e767774)

## Java provides three constant values in the Thread class:

**Thread.MIN_PRIORITY (1):** Lowest possible priority for a thread.
**Thread.NORM_PRIORITY (5):** Default priority assigned to a thread.
**Thread.MAX_PRIORITY (10):** Highest possible priority for a thread.