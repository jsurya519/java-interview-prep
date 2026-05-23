## Thread Pool
A Thread Pool is a collection of pre-created, reusable threads that are kept ready to perform tasks. Instead of creating a new thread every time you need to run something (which is costly in terms of memory and CPU), a thread pool maintains a fixed number of threads. When a task is submitted:

1. If a thread is free, it immediately picks up the task and runs it.
2. If all threads are busy, the task waits in a queue until a thread becomes available.
3. After finishing a task, the thread does not die. It goes back to the pool and waits for the next task.

**Thread Pool Initialization**
When we initialize a thread pool:
1. A fixed number of worker threads are created (e.g., 3).
2. These threads are kept idle, waiting for tasks.
3. A task queue is set up to hold submitted tasks until a worker is free.

**Thread Pool Working**
Here is the step by step working of thread pool.
**Step 1: Idle State**
1. Tasks are submitted and placed in the Task Queue.
2. Worker threads exist but are idle until work arrives.

<img width="800" height="400" alt="image" src="https://github.com/user-attachments/assets/db8a9cad-3273-4a7b-bb0c-6ae2156edb2d" />


**Step 2: Task Assignment**
1. Each idle thread picks a task from the queue.
2. Example: Thread 1 -> Task 1, Thread 2 -> Task 2, Thread 3 -> Task 3
3. Remaining tasks (Task 4, Task 5) wait in the queue.

<img width="800" height="400" alt="image" src="https://github.com/user-attachments/assets/08ee3889-fade-4409-9b12-4b41b4680b49" />



**Step 3: Thread Reuse**
1. Once a thread completes its current task, it becomes idle again.
2. It immediately takes the next waiting task from the queue.
3. Example: Thread 1 -> Task 4, Thread 2 -> Task 5, Thread 3 -> Idle (no tasks left)
   
<img width="800" height="400" alt="image" src="https://github.com/user-attachments/assets/000879d4-0b99-4cd1-9739-2b1c39c46803" />


```java
import java.util.concurrent.BlockingQueue;
import java.util.concurrent.LinkedBlockingQueue;

// Worker thread that executes tasks
class Worker extends Thread {
    private final BlockingQueue<Runnable> taskQueue;
    private static final Runnable POISON_PILL = () -> {};

    public Worker(BlockingQueue<Runnable> queue, String name) {
        super(name);
        this.taskQueue = queue;
    }

    public void run() {
        try {
            while (true) {
                Runnable task = taskQueue.take();

                if (task == POISON_PILL) {
                    break; // stop worker
                }

                try {
                    task.run();
                } catch (Exception e) {
                    System.out.println("Task error: " + e.getMessage());
                }
            }
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }
    }
}

// Thread Pool Implementation
class SimpleThreadPool {
    private final BlockingQueue<Runnable> taskQueue;
    private final Worker[] workers;
    private volatile boolean isShutdown = false;
    private static final Runnable POISON_PILL = () -> {};

    public SimpleThreadPool(int poolSize) {
        taskQueue = new LinkedBlockingQueue<>();
        workers = new Worker[poolSize];

        for (int i = 0; i < poolSize; i++) {
            workers[i] = new Worker(taskQueue, "Worker-" + (i + 1));
            workers[i].start();
        }
    }

    // Submit task
    public void submit(Runnable task) throws InterruptedException {
        if (!isShutdown) {
            taskQueue.put(task); // safer than offer()
        } else {
            throw new IllegalStateException("ThreadPool is shutdown");
        }
    }

    // Shutdown pool gracefully
    public void shutdown() {
        isShutdown = true;

        // Send stop signal to all workers
        for (int i = 0; i < workers.length; i++) {
            try {
                taskQueue.put(POISON_PILL);
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        }

        // Wait for workers to finish
        for (Worker worker : workers) {
            try {
                worker.join();
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        }
    }
}

// Test Program
public class ThreadPoolDemo {
    public static void main(String[] args) {
        SimpleThreadPool pool = new SimpleThreadPool(3);

        try {
            for (int i = 1; i <= 5; i++) {
                int taskId = i;

                pool.submit(() -> {
                    System.out.println("Executing Task " + taskId + " by " + Thread.currentThread().getName());
                    try {
                        Thread.sleep(1000);
                    } catch (InterruptedException e) {
                        Thread.currentThread().interrupt();
                    }
                });
            }
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }

        pool.shutdown();
        System.out.println("All tasks completed.");
    }
}
```

**Output:**

<img width="495" height="181" alt="image" src="https://github.com/user-attachments/assets/c9c8ed68-267f-4fc6-8ce9-678a82c5a1b7" />
