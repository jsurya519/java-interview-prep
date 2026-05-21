## Thread Lifecycle:

<img width="800" height="370" alt="image" src="https://github.com/user-attachments/assets/359af03a-8f5c-4d71-8217-2965a417ce11" />


In Java thread lifecycle, these are the most commonly discussed states:
```java
NEW → RUNNABLE → RUNNING (conceptually) → TERMINATED
```

Officially Java combines Runnable + Running into one state called RUNNABLE, but conceptually we separate them for understanding.

**1. NEW State**
A thread is in NEW state after thread object creation but before calling start().
```java
class MyThread extends Thread {

    public void run() {
        System.out.println("Child thread running");
    }
}

public class Test {

    public static void main(String[] args) {

        MyThread t = new MyThread();

        System.out.println(t.getState());
    }
}
// Output:
// NEW
```

**2. RUNNABLE State:**
After calling start(), thread enters RUNNABLE.

This means "Ready to run, waiting for CPU" or "Currently running on CPU". Java combines both into one state.

```java
class MyThread extends Thread {

    public void run() {

        for(int i = 1; i <= 5; i++) {
            System.out.println("Running...");
        }
    }
}

public class Test {

    public static void main(String[] args) {

        MyThread t = new MyThread();

        t.start();

        System.out.println(t.getState());
    }
}
// Output:
// RUNNABLE
```

**3. RUNNING State (Conceptual)**
Java does not provide a separate RUNNING state in Thread.State.
But conceptually:
1. when scheduler gives CPU to thread
2. thread is actually executing run()
we call it running.

```java
public void run() {
    System.out.println("Executing");
}
```

At this moment thread is conceptually running. But t.getState() still shows **RUNNABLE**.

