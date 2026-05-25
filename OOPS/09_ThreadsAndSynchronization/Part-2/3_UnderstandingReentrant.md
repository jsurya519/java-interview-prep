## Reentrant:
Reentrant means same thread can acquire the SAME lock multiple times without deadlock.

**Example using synchronized (monitor lock):**
```java
class Test {

    synchronized void method1() {
        System.out.println("method1");
        method2();
    }

    synchronized void method2() {
        System.out.println("method2");
    }
}
```

Suppose Thread-1 enters method1(), Now Thread-1 already holds monitor lock of current object (this).

Inside method1(), it calls method2(). method2() is also synchronized and needs SAME monitor lock.

If Java locks were NOT reentrant, Thread-1 would wait for its own lock forever and cause deadlo ck.

But Java allows same thread to re-acquire same lock. So execution succeeds.

