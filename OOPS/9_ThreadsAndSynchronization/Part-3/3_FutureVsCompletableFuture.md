## Future Vs CompletableFuture
Future and CompletableFuture are both used for asynchronous programming in Java, but CompletableFuture is much more powerful and flexible.

Future — Basic async result holder
CompletableFuture — Advanced async pipeline

## 1. Future Example
Simple async task with blocking get().

```java
import java.util.concurrent.*;

public class FutureExample {

    public static void main(String[] args) throws Exception {

        ExecutorService executor =
                Executors.newFixedThreadPool(2);

        Future<Integer> future = executor.submit(() -> {

            System.out.println("Task started...");
            Thread.sleep(3000);

            return 100;
        });

        System.out.println("Main thread doing other work...");

        // Blocking call
        Integer result = future.get();

        System.out.println("Result: " + result);

        executor.shutdown();
    }
}
// Output:
// Main thread doing other work...
// Task started...
// (waits 3 sec)
// Result: 100
```

**What happens internally**

```java
submit task
    ↓
task runs in another thread
    ↓
main thread continues
    ↓
future.get() waits
    ↓
result returned
```

Here, future.get() blocks the thread.

## 2. CompletableFuture Example
Same example using callback pipeline.
```java
import java.util.concurrent.*;

public class CompletableFutureExample {

    public static void main(String[] args) throws Exception {

        CompletableFuture<Integer> future =
                CompletableFuture.supplyAsync(() -> {

                    System.out.println("Task started...");

                    try {
                        Thread.sleep(3000);
                    } catch (Exception e) {
                    }

                    return 100;
                });

        future.thenApply(result -> result * 2)
              .thenAccept(finalResult ->
                      System.out.println("Final Result: " + finalResult));

        System.out.println("Main thread continues...");

        Thread.sleep(5000);
    }
}
// Output:
// Main thread continues...
// Task started...
// (after 3 sec)
// Final Result: 200
```

**Flow of CompletableFuture**
```java
run async task
    ↓
when result arrives
    ↓
multiply by 2
    ↓
print result
```
No manual blocking logic needed.

---
## Real Difference between Future and CompletableFuture

**Future style:**
```java
Integer result = future.get();
result = result * 2;
System.out.println(result);
```
You wait first

**CompletableFuture style:**
```java
future.thenApply(x -> x * 2)
      .thenAccept(System.out::println);
```
You define the pipeline first.

---
## One more example:
**Using Future (messy)**
```java
Future<String> emp = executor.submit(() -> "John");
Future<Integer> salary = executor.submit(() -> 50000);

String result =
        emp.get() + " earns " + salary.get();

System.out.println(result);
```
Here, problem is multiple blocking calls.

**Using CompletableFuture (clean)**
```java
CompletableFuture<String> emp =
        CompletableFuture.supplyAsync(() -> "John");

CompletableFuture<Integer> salary =
        CompletableFuture.supplyAsync(() -> 50000);

emp.thenCombine(salary,
        (e, s) -> e + " earns " + s)
   .thenAccept(System.out::println);

Thread.sleep(2000);
```

**Output:**
John earns 50000

---


| Future                    | CompletableFuture      |
| ------------------------- | ---------------------- |
| Basic async result holder | Full async pipeline    |
| Blocking `get()`          | Non-blocking callbacks |
| No chaining               | Supports chaining      |
| Hard to combine tasks     | Easy task composition  |
| Java 5                    | Java 8                 |
