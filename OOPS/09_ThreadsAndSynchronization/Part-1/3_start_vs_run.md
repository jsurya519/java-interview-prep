## start vs run:

**1. start() method**
The start() method is used to begin a new thread of execution. It performs two main tasks:
1. Allocates resources for a new thread.
2. Calls the run() method internally in the new thread.

```java
class MyThread extends Thread {

    public void run(){

        System.out.println("Thread running using start()");
    }

    public static void main(String[] args)
    {
        MyThread t1 = new MyThread();

        // Starts a new thread
        t1.start();
    }
}
// Output:
// Thread running using start()
```

**2. run() method:**
The run() method contains the code executed by the thread. However, calling run() directly does not create a new thread. Instead, it behaves like a normal method call executed in the current thread.

```java
class MyThread extends Thread{

    public void run(){

        System.out.println("Thread running using run()");
    }

    public static void main(String[] args){

        MyThread t1 = new MyThread();

        // Does NOT start a new thread
        t1.run();
    }
}
// Output:
// Thread running using run()
```

## Multiple Invocations:
We can't call the start() method twice otherwise it will throw an IllegalThreadStateException whereas run() method can be called multiple times as it is just a normal method calling.

```java
class MyThread extends Thread {

    public void run(){

        System.out.println(
            "Current thread: "
            + Thread.currentThread().getName());
    }
}

public class Geeks{

    public static void main(String[] args){

        MyThread t = new MyThread();
        t.start();
        t.start(); // Throws exception
    }
}
// Output:
// Current thread: Thread-0
// Exception in thread "main" java.lang.IllegalThreadStateException
```

```java
class MyThread extends Thread {

    public void run(){

        System.out.println(
            "Current thread: "
            + Thread.currentThread().getName());
    }
}

public class Geeks{

    public static void main(String[] args){

        MyThread t = new MyThread();
        t.run();
        t.run(); // Works fine
    }
}
// Output:
// Current thread: main
// Current thread: main
```

As we can see in the above example, calling run() method twice doesn't raise any exception and it is executed twice as expected but on the main thread itself.

## start() vs run() method:

| Feature | start() Method | run() Method |
|---|---|---|
| **Thread Creation** | Creates a new thread. | Does not create a new thread. |
| **Execution Context** | Runs `run()` in a separate thread. | Runs in the current thread. |
| **Purpose** | Starts concurrent execution. | Defines the task/code to execute. |
| **Behavior** | Allows parallel execution. | Acts like a normal method call. |
| **Usage Example** | `t1.start();` | `t1.run();` |

