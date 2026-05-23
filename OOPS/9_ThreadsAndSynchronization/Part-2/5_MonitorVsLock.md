## Monitor vs Lock
1. Both are used to achieve synchronization in concurrent programming
2. Monitor is implicit (synchronized), while Lock is explicit and more flexible
3. Lock provides advanced features compared to basic Monitor functionality

## Monitor:
1. It ensures that only one thread can access a synchronized block or method at a time.
2. Every Java object has an intrinsic lock
3. Only one thread can hold the monitor lock at a time; others must wait.
4. Used with synchronized, wait(), notify(), and notifyAll() for thread communication.

```java
import java.io.*;

class SharedDataPrinter
{
    // Monitor implementation is carried on by Using synchronous method
	synchronized public void display(String str)
	{

		for (int i = 0; i < str.length(); i++)
        {
			System.out.print(str.charAt(i));

			// Try-catch block for exceptions because sleep() method is used
			try
            {
			// Making thread to sleep for nanoseconds as passed in the arguments Thread.sleep(100);
			}
			catch (Exception e) {
		 }
 	  }
   }
}

class Thread1 extends Thread {

	SharedDataPrinter p;

	public Thread1(SharedDataPrinter p)
	{

		this.p = p;
	}

	public void run()
	{

		p.display("Geeks");
	}
}

class Thread2 extends Thread {

	SharedDataPrinter p;

	public Thread2(SharedDataPrinter p) { this.p = p; }

	public void run()
	{

		p.display(" for Geeks");
	}
}

class Geeks
{
	public static void main(String[] args)
	{
		SharedDataPrinter printer = new SharedDataPrinter();

		Thread1 t1 = new Thread1(printer);
		Thread2 t2 = new Thread2(printer);

		t1.start();
		t2.start();
	}
}

```

## Lock:
```java
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class Geeks {

    // Shared resource accessed by multiple threads
    private static int sharedResource = 0;

    // ReentrantLock for thread synchronization
    private static final Lock lock = new ReentrantLock();

    public static void main(String[] args) {

        // Creating two threads to increment the shared resource
        Thread t1 = new Thread(new IncrementTask());
        Thread t2 = new Thread(new IncrementTask());

        // Start both threads
        t1.start();
        t2.start();

        try {
            // Wait for both threads to complete
            t1.join();
            t2.join();
        } catch (InterruptedException e) {
            System.out.println("Thread interrupted");
        }

        // Print final value of shared resource
        System.out.println("Final value of sharedResource: "
                           + sharedResource);
    }

    // Task to increment the shared resource
    static class IncrementTask implements Runnable {
        @Override
        public void run() {
            for (int i = 0; i < 1000; i++) {

                // Acquire the lock
                lock.lock();
                try {
                    sharedResource++;
                } finally {

                    // Release the lock
                    lock.unlock();
                }
            }
        }
    }
}
```

## Lock vs Monitor
| Aspect | Monitor (`synchronized`) | Lock (`ReentrantLock`) |
|---|---|---|
| Origin | JVM intrinsic, low-level primitive | Java 5, high-level API |
| Implementation | Implicit, JVM-managed | Explicit, programmer-managed |
| Critical Section Management | Automatic | Manual (`lock()` / `unlock()`) |
| Thread Queueing | JVM manages waiting threads | Programmer can choose fairness policies |
| Features | Simple mutual exclusion | Advanced: fairness, interruptible, `tryLock`, multiple conditions |
| Performance | Lightweight for small threads | Slightly higher overhead, but more flexible |
| Usage | Simple synchronization, small thread pools | Complex synchronization, high concurrency scenarios |
| Deadlock Handling | Less control | More explicit control but can still occur |