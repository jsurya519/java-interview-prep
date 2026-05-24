## String vs StringBuilder vs StringBuffer

**String**
A String in Java is an immutable sequence of characters, meaning once a String object is created, it cannot be changed.
```java
public class StringExample{

    public static void main(String[] args){

        String str = "Hello";
        // creates a new object
        str.concat(" World");
        System.out.println(str);
    }
}
// Output:
// Hello
```

**StringBuilder**
StringBuilder is a mutable sequence of characters. It allows modification of the string content without creating new objects.

```java
public class Geeks{

    public static void main(String[] args){

        StringBuilder sb = new StringBuilder("Hello");
        sb.append(" World");
        System.out.println(sb);
    }
}
// Output:
// Hello World
```

**StringBuffer**
StringBuffer is also a mutable sequence of characters, similar to StringBuilder, but it is thread-safe and synchronized.

```java
public class Geeks{

    public static void main(String[] args){

        StringBuffer sb = new StringBuffer("Hello");
        sb.append(" World");
        System.out.println(sb);
    }
}
// Output:
// Hello World
```

## Use Cases of String, StringBuilder, and StringBuffer
1. If a string is going to remain constant throughout the program, then use the String class object because a String object is immutable.
2. If a string can change (for example: lots of logic and operations in the construction of the string) and will only be accessed from a single thread, using a StringBuilder is good enough.
3. If a string can change and will be accessed from multiple threads, use a StringBuffer because StringBuffer is synchronous, so you have thread-safety.

# String vs StringBuilder vs StringBuffer

| Feature | String | StringBuilder | StringBuffer |
|---|---|---|---|
| **Mutability** | Immutable (creates new objects on modification) | Mutable (modifies in place) | Mutable (modifies in place) |
| **Thread-Safe** | Yes | No | Yes |
| **Performance** | Slower because it creates a new object each time | Faster (no extra object creation) | Slower due to synchronization overhead |
| **Use Case** | Fixed, unchanging strings | Single-threaded string manipulation | Multi-threaded string manipulation |

---