## Memory Leaks
Memory leaks in Java occur when objects that are no longer needed remain referenced, preventing garbage collection. Over time, this increases memory usage, degrades performance, and can eventually crash the application.
1. Java provides automatic garbage collection, but it cannot remove objects that are still referenced.
2. Poor reference management is the main reason memory leaks occur in Java applications.

![7ae0d359ea0b63ee0de17e41540a5193.png](:/594ff5f29cb748a894fbf3d40562d72d)

**Note:** If an object is no longer needed, it is important to remove references to it so the garbage collector can free its memory.

## Why Do Memory Leaks Happen in Java?
Even though Java has automatic garbage collection, memory leaks can occur when the program keeps references to objects that are no longer needed, preventing their removal and wasting memory.

```java
import java.util.ArrayList;
import java.util.List;

public class GFG{

    public static void main(String[] args){

        List<byte[]> list = new ArrayList<>();

        while (true) {
            // Each iteration creates a 1 MB object
            list.add(new byte[1024 * 1024]);
        }
    }
}
```

![521af26f575f193d6e61ca042b5b199a.png](:/ea3b6211cd7e4e39b5cfe31e5c8b86ec)

## How to Avoid Memory Leaks?
1. Stop keeing things that we do not need in our program do not initialize unnecessary variables and list items to null.
2. Do not let lists or caches keep growing forever without removing old stuff.
3. It is always recommended to close files and database connections when we are done with them.

