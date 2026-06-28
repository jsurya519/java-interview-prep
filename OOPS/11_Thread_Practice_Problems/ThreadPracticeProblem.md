# Java Concurrency Practice Roadmap

## Goal

Become comfortable writing concurrent Java code during interviews by implementing progressively harder problems.

---

# Level 1 - Thread Basics (1-5)

## 1. Print Numbers Using Two Threads

**Problem**

Print numbers from 1 to N using two threads.

Example:

```
Thread A -> 1 3 5 7 ...
Thread B -> 2 4 6 8 ...
```

**Concepts**

* Thread creation
* `synchronized`
* `wait()`
* `notify()`

---

## 2. Producer Consumer (Single Producer & Consumer)

Implement a bounded queue.

Producer inserts data.

Consumer removes data.

**Concepts**

* `wait()`
* `notify()`
* Blocking Queue fundamentals

---

## 3. Producer Consumer (Multiple Producers & Consumers)

Extend the previous problem.

Example:

* 3 Producers
* 5 Consumers

Avoid race conditions and data corruption.

**Concepts**

* Lock contention
* `notifyAll()`
* Shared resource synchronization

---

## 4. Thread-safe Counter

Implement:

* `increment()`
* `decrement()`
* `get()`

Test with:

* 100 Threads
* Each thread increments 10,000 times

Expected Result:

```
1,000,000
```

**Concepts**

* `synchronized`
* `AtomicInteger`
* `ReentrantLock`

---

## 5. Implement CountDownLatch

Do **not** use Java's built-in `CountDownLatch`.

Implement:

* `await()`
* `countDown()`

**Concepts**

* Thread coordination
* Condition waiting

---

# Level 2 - Locking (6-10)

## 6. Implement Read Write Lock

Requirements:

* Multiple readers allowed
* Only one writer
* Readers blocked while writing

**Concepts**

* Reader count
* Exclusive locking
* Fairness

---

## 7. Dining Philosophers

Implement the classic concurrency problem.

Avoid:

* Deadlock
* Starvation

---

## 8. Sleeping Barber Problem

Implement:

* Waiting room
* Barber chair
* Customer queue

**Concepts**

* Producer-Consumer variation
* Thread coordination

---

## 9. Traffic Signal

Multiple roads.

Only one road should have a green signal at a time.

Cars on other roads must wait.

---

## 10. Parking Lot

Requirements:

* Multiple entry gates
* Multiple exit gates
* Shared parking slots

Ensure slot count is always correct.

---

# Level 3 - Concurrent Collections (11-15)

## 11. Thread-safe HashMap

Implement:

* `put()`
* `get()`
* `remove()`

Do **not** use `ConcurrentHashMap`.

---

## 12. Thread-safe LRU Cache

Implement:

* `put()`
* `get()`
* LRU eviction

Support concurrent access safely.

---

## 13. Thread-safe TTL Cache

Implement:

* `put()`
* `get()`
* Automatic expiration
* Background cleanup thread

Support concurrent readers and writers.

---

## 14. Fixed Size Cache

Requirements:

* Maximum capacity
* Remove oldest entry when full
* Thread-safe implementation

---

## 15. Concurrent Statistics Collector

Implement:

* `record(int value)`

Support:

* `average()`
* `max()`
* `min()`
* `count()`

Handle concurrent updates safely.

---

# Level 4 - Executors & Thread Pools (16-20)

## 16. Build Your Own Thread Pool

Implement:

* `submit()`
* `shutdown()`

Worker threads continuously process submitted tasks.

---

## 17. Scheduled Executor

Support:

```
Execute task after N seconds
```

---

## 18. Rate Limiter

Implement:

* Token Bucket

OR

* Leaky Bucket

Example:

```
Allow only 10 requests per second
```

---

## 19. Task Queue

Implement:

* Multiple producers
* Worker thread pool
* FIFO execution

---

## 20. Async File Processor

Requirements:

* Process multiple files simultaneously
* Fixed thread pool
* Return processing results

---

# Level 5 - Real Interview Problems (21-25)

## 21. URL Shortener

Support concurrent requests.

Implement:

* `shorten()`
* `lookup()`

Avoid duplicate IDs.

---

## 22. Bank Account

Implement:

* `deposit()`
* `withdraw()`
* `transfer()`

Avoid:

* Race conditions
* Deadlocks

---

## 23. Order Management

Implement concurrent order processing.

Example:

* Reserve inventory
* Process payment
* Ship order

Prevent overselling.

---

## 24. Ticket Booking System

Example:

```
100 Seats

1000 Concurrent Booking Requests
```

Each seat should be booked only once.

---

## 25. Inventory Management

Implement:

* Add stock
* Remove stock

Inventory should never become negative.

---

# Level 6 - Advanced (26-30)

## 26. Implement BlockingQueue

Do **not** use Java's `BlockingQueue`.

Implement:

* `put()`
* `take()`

---

## 27. Implement ConcurrentHashMap (Simplified)

Use:

* Bucket Locking

OR

* Segment Locking

---

## 28. Thread-safe Logger

Requirements:

* Multiple threads writing logs
* No corruption
* Preserve ordering

---

## 29. Mini Message Broker

Implement:

* Producers
* Topics
* Subscribers
* Message offsets

Support multiple concurrent publishers and subscribers.

---

## 30. Distributed Cache (Single JVM Simulation)

Features:

* Thread-safe
* TTL
* LRU eviction
* Background cleanup
* Concurrent reads and writes

---

# Bonus Problems

These frequently appear in coding interviews:

* Print FooBar Alternately
* Print Zero Even Odd
* Multithreaded FizzBuzz
* Building H₂O (Semaphore Problem)
* Uber Ride Matching
* Multithreaded Web Crawler
* Parallel Merge Sort
* Parallel File Downloader
* Web Server with Thread Pool

---

# Suggested Learning Plan

| Week   | Topics                                    |
| ------ | ----------------------------------------- |
| Week 1 | Problems 1-5 (Thread Basics)              |
| Week 2 | Problems 6-10 (Locks & Coordination)      |
| Week 3 | Problems 11-15 (Concurrent Collections)   |
| Week 4 | Problems 16-20 (Executors & Thread Pools) |
| Week 5 | Problems 21-25 (Real Interview Scenarios) |
| Week 6 | Problems 26-30 (Advanced Concurrency)     |

---

# Practice Rules

For every problem:

* Write the solution from scratch.
* Test with multiple threads.
* Intentionally create race conditions and fix them.
* Measure correctness before optimizing.
* Try both `synchronized` and `ReentrantLock` implementations where applicable.
* Compare with Java's built-in concurrent utilities after completing your implementation.

**Goal:** Become confident implementing concurrent systems under interview conditions without relying on memorized solutions.
