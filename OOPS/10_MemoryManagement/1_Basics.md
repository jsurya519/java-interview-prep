## Memory Management
1. Java Virtual Machine (JVM) automatically handles the allocation and deallocation of memory.
2. It uses a garbage collection to reclaim memory by removing unused objects, eliminating the need for manual memory management.

## JVM memory structure

<img width="660" height="411" alt="image" src="https://github.com/user-attachments/assets/5d85c60a-af7b-42c6-a4e1-137b346bdb89" />


**1. Heap Area**
1. It is created when the JVM starts.JVM allows user to adjust the heap size.
2. When the new keyword is used the object is allocated in the heap and its reference is stored in the stack.
3. There exists one and only one heap for a running JVM process.
4. Garbage collection in heap area is mandatory.

```java
Scanner sc = new Scanner(System.in)
```

Here, the Scanner object is stored in the heap and the reference sc is stored in the stack.

**Heap Memory is divided into several regions:**

**Young Generation:** The area within the heap where new objects are generally allocated.

**Old Generation:** The area within the heap where long-lived metadata Objects are stored after surviving multiple garbage collection cycles.

**Permanent Generation:** It stores metadata about classes and methods in Java 7 and earlier versions.

Starting with Java 8, the old and permanent generation space was removed and replaced by metaspace. This change helps Java to handle class metadata better and also reduces the chances of errors caused by permanent generation.

**2. Method Area(Metaspace in modern JVMs)**
1. Method area is used to store class-level information such as class structures, Method bytecode, Static variables, Constant pool, Interfaces. Here, the constant pool is nothing but Runtime constant pool that is inside the method area where String literals of class such as class/interface name, field name, method names, numeric constants, etc are stored.
2. Method area can be of fixed or dynamic size depending on the system's configuration.
3. Garbage collection of the method area is not guaranteed and depends on JVM implementation.

**Example:**
```java
class Employee {

    static String company = "ABC";

    int id;
    String name;

    public void display() {
        System.out.println(id + " " + name);
    }
}
```

When JVM loads Employee.class, it stores metadata like:
```java
Class Name        -> Employee
Superclass        -> Object
Field Names       -> id, name, company
Method Names      -> display()
Method Bytecode   -> compiled code of display()
Static Variables  -> company
Runtime Constant Pool
```


```java
import java.io.*;

class Geeks {

    // static variables are stored in the Method Area
    static int v = 100;

    // instance variables are stored in the Heap
    int i = 10;

    public void Display()
    {
        // local variables are stored in the Stack
        int s = 20;

        System.out.println(v);
        System.out.println(s);
    }
}

public class Main {
    public static void main(String[] args) {
        Geeks g = new Geeks();

        // Calling the Display method
        g.Display();
    }
}
```

**Note:**
1. Static variables are stored in the Method Area.
2. Instance variables are stored in the Heap.
3. Local variables are stored in the stack.

**3. JVM Stacks**
1. A stack is created when a thread is created and the JVM stack is used to store method execution data, including local variables, method arguments and return addresses.
2. Each Thread has its own stack, ensuring thread safety.
3. Stacks size can be either fixed or dynamic and it can be set when the stack is created.
4. The memory for stack needs not to be contiguous.
5. Once a method completes execution, its associated stack frame is removed automatically.

**4. Native Method Stacks**
1. Native method stack is also known as C stacks.
2. Native method stacks handle the execution of native methods that interact with the Java code.

**5. Program Counter (PC) Registers**
1. Each thread has its own PC register.
2. The PC Register stores the address/location of the current instruction being executed by that thread.

"Which line of bytecode should this thread execute next?"

# Heap vs Stack in Java

| Feature | Heap | Stack |
|---|---|---|
| Storage | Stores objects and instance variables | Stores method calls and local variables |
| Size & Speed | Larger but slower | Smaller but faster |
| Access Pattern | Random access | LIFO (Last-In-First-Out) |
| Memory Management | Allocation is manual (`new` keyword), deallocation is automatic (Garbage Collector) | Allocation and deallocation are automatic |
| Lifetime | Long-lived | Short-lived (method duration) |
| Thread Sharing | Shared among all threads (requires synchronization) | Each thread has its own stack (thread-safe) |

---

