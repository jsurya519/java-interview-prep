## Types of Garbage Collectors
**1. Serial Garbage Collector**
The Serial Garbage Collector is the simplest GC and uses a single thread for all garbage collection tasks. It pauses all application threads during GC and is best suited for small applications or systems with limited memory.

**2. Parallel Garbage Collector**
The Parallel Garbage Collector (default in Java 8) uses multiple threads to perform garbage collection, improving throughput. It pauses the application during GC but provides better performance than Serial GC and is ideal for high-throughput applications.

**3. Concurrent Mark-Sweep Collector**
CMS is designed to minimize pause times by performing most GC work concurrently with application threads. It is suitable for low-latency applications like web servers but is deprecated in newer Java versions.

**4. G1 Garbage Collector**
G1 GC divides the heap into multiple regions and collects regions with the most garbage first. It provides predictable pause times, supports large heaps, and became the default GC in Java 9.

**5. Z Garbage Collector:**
ZGC is a low-latency garbage collector introduced in Java 11. It handles very large heaps with pause times typically in milliseconds, making it suitable for performance-critical applications.

**6. Shenandoah Garbage Collector**
Shenandoah, introduced in Java 12, focuses on very low pause times by running garbage collection concurrently with application threads. It is ideal for latency-sensitive systems.

