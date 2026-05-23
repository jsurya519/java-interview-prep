## Executor Framework:
 It lets developers submit tasks without manually creating or controlling threads, as the framework handles scheduling and execution.

 ![f02956be463b585e163c4ea3967bd67b.png](:/cc9a0eb4f9b145cb86ffdb87607995ab)

##  Common Types of Executors:

| Executor Type | Description | Syntax Example |
|---|---|---|
| `SingleThreadExecutor` | Creates a thread pool with a single thread that executes tasks sequentially. | `ExecutorService executor = Executors.newSingleThreadExecutor();` |
| `FixedThreadPool` | Creates a pool with a fixed number of threads. Excess tasks are queued until a thread becomes available. | `ExecutorService pool = Executors.newFixedThreadPool(2);` |
| `CachedThreadPool` | Creates threads as needed and reuses idle ones. Suitable for many short-lived asynchronous tasks. | `ExecutorService pool = Executors.newCachedThreadPool();` |
| `ScheduledThreadPool` | Executes tasks periodically or after a specified delay. | `ScheduledExecutorService scheduler = Executors.newScheduledThreadPool(1);` |


```java
import java.util.concurrent.*;

class Tasks implements Runnable {

    @Override
    public void run()
    {
        System.out.println("Hello world");
    }
}

public class Dummy {

    public static void main(String[] args)
    {

        Tasks task = new Tasks();

        // Creating object of ExecutorService class and Future object Class
        ExecutorService executorService = Executors.newFixedThreadPool(4);
        executorService.submit(task);

        executorService.submit(task);

        // Cleaning resource and shutting down JVM by saving JVM state using shutdown() method
        executorService.shutdown();
    }
}

// Output:
// Hello world
// Hello world
```


**ScheduledThreadPool Example:**
```java
import java.util.concurrent.Executors;
import java.util.concurrent.ScheduledExecutorService;
import java.util.concurrent.TimeUnit;

public class Main {

    public static void main(String[] args) {

        ScheduledExecutorService scheduler =
                Executors.newScheduledThreadPool(1);

        // Run task after 3 seconds
        scheduler.schedule(() -> {
            System.out.println("Task executed");
        }, 3, TimeUnit.SECONDS);

        System.out.println("Main thread continues...");

        // Shutdown after some delay
        scheduler.shutdown();
    }
}
```

**Callable Example:**
```java
import java.util.concurrent.*;

class Task implements Callable<String> {

    private String message;

    public Task(String message)
    {
        this.message = message;
    }

    public String call() throws Exception
    {
        return "Hi " + message + "!";
    }
}

public class Geeks {

    public static void main(String[] args)
    {

        Task task = new Task("GeeksForGeeks");

        // Creating object of ExecutorService class and Future object Class
        ExecutorService executorService = Executors.newFixedThreadPool(4);
        Future<String> result = executorService.submit(task);

        // Try block to check for exceptions
        try {
            System.out.println(result.get());
        }

        // Catch block to handle the exception
        catch (InterruptedException  | ExecutionException e) {

            System.out.println("Error occurred while executing the submitted task");
            e.printStackTrace();
        }

        // Cleaning resource and shutting down JVM by saving JVM state using shutdown() method
        executorService.shutdown();
    }
}
```