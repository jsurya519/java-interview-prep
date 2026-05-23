## ReentrantLock features
It provides a more flexible mechanism for thread synchronization compared to the synchronized keyword.
**Reentrancy:** The same thread can acquire the lock multiple times. Each lock acquisition must be paired with a corresponding unlock.
**Explicit Locking:** Unlike synchronized, Reentrant Lock requires manual locking and unlocking using lock() and unlock().
**Interruptible:** Threads waiting for a lock can be interrupted.
**TryLock() Support:** Threads can attempt to acquire a lock without waiting indefinitely.
**Fairness Policy:** Locks can be configured to grant access in first-come-first-serve order.

```java
import java.util.concurrent.locks.ReentrantLock;

public class SharedResource {
    private final ReentrantLock lock = new ReentrantLock();

    public void performTask() {
        lock.lock();  // Acquire lock
        try {
            // Critical section code
            System.out.println(Thread.currentThread().getName() + " is performing task");
        } finally {
            lock.unlock();  // Release lock
        }
    }
}
```


**Reentrant Behavior Example**
```java
import java.util.concurrent.locks.ReentrantLock;

public class ReentrantExample {
    private final ReentrantLock lock = new ReentrantLock();

    public void methodA() {
        lock.lock();
        try {
            System.out.println("Inside Method A");
            methodB();  // Reentrant lock allows the same thread to enter methodB
        } finally {
            lock.unlock();
        }
    }

    public void methodB() {
        lock.lock();
        try {
            System.out.println("Inside Method B");
        } finally {
            lock.unlock();
        }
    }

    public static void main(String[] args) {
        ReentrantExample example = new ReentrantExample();
        example.methodA();
    }
}
```

## Fairness Policy
Reentrant locks can be created with a fairness option:
```java
ReentrantLock fairLock = new ReentrantLock(true); // Fair lock
```

**Fair lock:** Threads acquire the lock in the order they requested it (first-come-first-serve).
**Non-fair lock (default):** Threads may acquire the lock in an arbitrary order, which can improve throughput but may cause starvation.

## Advantages over synchronized
| Feature | `synchronized` | `ReentrantLock` |
|---|---|---|
| Lock acquisition | Implicit | Explicit (`lock()` and `unlock()`) |
| Interruptible | No | Yes |
| Try-lock | No | Yes (`tryLock()`) |
| Fairness | No | Optional |
| Reentrant | Yes | Yes |


