## Threads Basics:

**What is a program?**

A program is passive code on disk.

**What is a process?**

A process is that program in execution.

**Example:**

chrome.exe on disk → program

Opened Chrome browser → process

**Example in java:**

MyApp.class → program

java MyApp running → process

**What is a thread?**

A thread is the smallest unit of execution inside a process.

**In Java:**
Process = entire Java application (JVM)
Thread = path of execution doing some task inside that application

Threads run inside a process and execute program instructions.

---

In normal Java programs like:
```java
public class Test {
    public static void main(String[] args) {
        System.out.println("Hello");
    }
}
```
you are mainly writing code executed by the main thread. The JVM automatically creates this thread and starts execution from main method.

But internally, JVM already uses multiple threads even if you never create one.

**Common JVM threads:**
1. main thread
2. Garbage Collector thread
3. JIT compiler thread
4. Finalizer/Cleanup threads

---

**What is thread?**
A thread is a lightweight, independent unit of execution inside a program (process).

**Multithreading:**
1. Multithreading in Java is a feature that enables a program to run multiple threads simultaneously, allowing tasks to execute in parallel and utilize the CPU more efficiently.
2. Threads allow parallel execution of tasks.
3. A process can have multiple threads.
4. Each thread runs independently but shares the same memory.

