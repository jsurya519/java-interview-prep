## Data Inconsistency:

Imagine two threads are trying to increment the same variable at the same time.
```java
class Counter {

    int count = 5;

    void increment() {
        count++;
    }
}
```

Now suppose:
Thread-1 calls increment()
Thread-2 also calls increment()

You may expect count value = 7.

But sometimes it becomes 6. Why?

Because count++ is not a single operation internally. It actually happens in 3 steps:
```java
1. Read count
2. Add 1
3. Write back
```

| Step | Thread-1   | Thread-2   | count |
| ---- | ---------- | ---------- | ----- |
| 1    | Reads 5    |            | 5     |
| 2    |            | Reads 5    | 5     |
| 3    | Adds 1 → 6 |            | 5     |
| 4    |            | Adds 1 → 6 | 5     |
| 5    | Writes 6   |            | 6     |
| 6    |            | Writes 6   | 6     |


At the end, both threads updates to 6.  One increment is lost. This problem is called Data inconsistency. Because multiple threads are accessing and modifying shared data simultaneously.

```java
class Counter {

    int count = 0;

    void increment() {
        count++;
    }
}

public class Test {

    public static void main(String[] args) throws Exception {

        Counter c = new Counter();

        Thread t1 = new Thread(() -> {
            for(int i = 0; i < 1000; i++) {
                c.increment();
            }
        });

        Thread t2 = new Thread(() -> {
            for(int i = 0; i < 1000; i++) {
                c.increment();
            }
        });

        t1.start();
        t2.start();

        t1.join();
        t2.join();

        System.out.println(c.count);
    }
}
```

Expected output: 2000

Actual output may be:
1843
1972
1991
2000

Different every run.

To solve this inconsistency, Java provides synchronization:
```java
synchronized void increment() {
    count++;
}
```

Now only one thread can execute increment() at a time, so result will consistently be:
2000