## Thread Yield
yield() is a static method that gives a hint to the thread scheduler that:
"Current thread is willing to pause and allow other runnable threads to execute."

It does:

NOT release locks

NOT move thread to waiting state

NOT guarantee context switch

Thread usually remains in RUNNABLE state.

```java
class MyThread extends Thread {

    public void run() {

        for(int i = 1; i <= 3; i++) {

            System.out.println(getName() + " : " + i);

            Thread.yield();
        }
    }
}

public class Main {

    public static void main(String[] args) {

        MyThread t1 = new MyThread();
        MyThread t2 = new MyThread();

        t1.start();
        t2.start();
    }
}
```

**Possible outputs:**
```java
Thread-0 : 1
Thread-1 : 1
Thread-0 : 2
Thread-1 : 2
Thread-0 : 3
Thread-1 : 3
```
or
```java
Thread-0 : 1
Thread-0 : 2
Thread-0 : 3
Thread-1 : 1
Thread-1 : 2
Thread-1 : 3
```


## Flow
```java
Thread T1 running
       ↓
Thread.yield()
       ↓
T1 says:
"I can pause if others need CPU"
       ↓
Scheduler decision:
   → continue T1
   OR
   → run another thread T2
       ↓
Later T1 resumes after yield()
```
