## Java Strings
A String in Java is an object used to store a sequence of characters. It is immutable.

## Ways of Creating a Java String
There are two ways to create a string in Java:

**1. String literal (Static Memory)**
In Java, the String Constant Pool (SCP) is a special area inside the heap memory where string literals are stored. To make Java more memory efficient (because no new objects are created if it exists already in the string constant pool).

<img width="800" height="400" alt="image" src="https://github.com/user-attachments/assets/79e244fb-cf85-415a-a350-28ac52c7712c" />


<img width="800" height="400" alt="image" src="https://github.com/user-attachments/assets/061f9d3a-4616-466c-ac43-695319e51f38" />


**2. Using new keyword (Heap Memory)**
Using the new keyword creates a new object in heap memory, even if the same string already exists in the pool.
1. One object is created in the heap memory
2. The string literal is stored in the string pool (if not already present)
3. The reference variable points to the heap object, not the pool

<img width="800" height="400" alt="image" src="https://github.com/user-attachments/assets/8f6ecc3e-65a4-4692-baca-0820eb5f07cc" />



## Commonly Used Java String Methods
**1. int length() Method**
```java
public class Geeks {
    public static void main(String[] args) {
        String s = "Hello, World!";
        System.out.println(s.length());
    }
}
// Output:
// 13
```

**2. charAt(int i) Method**
```java
public class Geeks {
    public static void main(String[] args) {
        String s = "Hello, World!";
        System.out.println(s.charAt(7));
    }
}
// Output:
// W
```

**3. String substring(int i) Method**
```java
public class Geeks {
    public static void main(String[] args) {
        String s = "Hello, World!";
        System.out.println(s.substring(7));
    }
}
// Output:
// World!
```

**4. String substring(int i, int j) Method**
```java
public class Geeks {
    public static void main(String[] args) {
        String s = "Hello, World!";
        System.out.println(s.substring(7, 12));
    }
}
// Output:
// World
```

**5. String concat( String str) Method**
```java
public class Geeks {
    public static void main(String[] args) {
        String s = "Hello, World!";
        System.out.println(s.concat("!!!"));
    }
}
// Output:
// Hello, World!!!!
```

**6. int indexOf(String s) Method**
```java
public class Geeks {
    public static void main(String[] args) {
        String s = "Hello, World!";
        System.out.println(s.indexOf("World"));
    }
}
// Output:
// 7
```

**7. int indexOf(String s, int i) Method**
```java
public class Geeks {
    public static void main(String[] args) {
        String str = "Hello, World!";
        System.out.println(str.indexOf("l", 4));
    }
}
// Output:
// 10
```

**8. int lastIndexOf(String s) Method**
```java
public class Geeks {
    public static void main(String[] args) {
        String s = "Hello, World!";
        System.out.println(s.lastIndexOf("l"));
    }
}
// Output:
// 10
```

**9. boolean equals(Object otherObj) Method**
```java
public class Geeks {
    public static void main(String[] args) {
        String s = "Hello, World!";
        System.out.println(s.equals("Hello, World!"));
    }
}
// Output:
// true
```

**10. boolean equalsIgnoreCase(String anotherString) Method**
```java
public class Geeks {
    public static void main(String[] args) {
        String s = "Hello, World!";
        System.out.println(s.equalsIgnoreCase("hello, world!"));
    }
}
// Output:
// true
```

**11. int compareTo(String anotherString) Method**
```java
public class Geeks {
    public static void main(String[] args) {
        String s = "Hello, World!";
        System.out.println(s.compareTo("Hello, Java!"));
    }
}
// Output:
// 13
```

**12. int compareToIgnoreCase(String anotherString) Method**
```java
public class Geeks {
    public static void main(String[] args) {
        String s = "Hello, World!";
        System.out.println(s.compareToIgnoreCase("hello, java!"));
    }
}
// Output:
// 13
```

**13. String toLowerCase() Method**
```java
public class Geeks {
    public static void main(String[] args) {
        String s = "Hello, World!";
        System.out.println(s.toLowerCase());
    }
}
// Output:
// hello, world!
```

**14. String toUpperCase() Method**
```java
public class Geeks {
    public static void main(String[] args) {
        String s = "Hello, World!";
        System.out.println(s.toUpperCase());
    }
}
// Output:
// HELLO, WORLD!
```

**15. String trim() Method**
```java
public class Geeks {
    public static void main(String[] args) {
        String s = "   Hello, Trim!   ";
        System.out.println("'" + s.trim() + "'");
    }
}
// Output:
// 'Hello, Trim!'
```

**16. String replace(char oldChar, char newChar) Method**
```java
public class Geeks {
    public static void main(String[] args) {
        String s = "Hello, World!";
        System.out.println(s.replace('l', 'x'));
    }
}
// Output:
// Hexxo, Worxd!
```

**17. boolean contains(CharSequence sequence) Method**
```java
public class Geeks {
    public static void main(String[] args) {
        String s = "Hello, World!";
        System.out.println(s.contains("World"));
    }
}
// Output:
// true
```

**18. char[] toCharArray() Method**
```java
public class Geeks {
    public static void main(String[] args) {
        String str = "Hello";
        char[] chars = str.toCharArray();
        for(char c : chars) {
            System.out.print(c + " ");
        }
    }
}
// Output:
// H e l l o
```

**19. boolean startsWith(String prefix) Method**
```java
public class Geeks {
    public static void main(String[] args) {
        String s = "Hello, World!";
        System.out.println(s.startsWith("Hello"));
    }
}
// Output:
// true
```


| Method | Description |
|---|---|
| `int length()` | Returns the number of characters in the String. |
| `char charAt(int i)` | Returns the character at ith index. |
| `String substring(int i)` | Returns the substring from the ith index character to the end. |
| `String substring(int i, int j)` | Returns the substring from index `i` to `j-1`. |
| `String concat(String str)` | Concatenates the specified string to the end of this string. |
| `int indexOf(String s)` | Finds the first occurrence of the given substring. Returns `-1` if not found. |
| `int indexOf(String s, int i)` | Returns the first occurrence of the specified string starting from index `i`. |
| `int lastIndexOf(String s)` | Returns the last occurrence of the specified string. Returns `-1` if not found. |
| `boolean equals(Object otherObj)` | Compares this string to the specified object. |
| `boolean equalsIgnoreCase(String anotherString)` | Compares two strings ignoring case considerations. |
| `int compareTo(String anotherString)` | Compares two strings lexicographically. |
| `int compareToIgnoreCase(String anotherString)` | Compares two strings lexicographically ignoring case considerations. |
| `String toLowerCase()` | Converts all characters in the string to lowercase. |
| `String toUpperCase()` | Converts all characters in the string to uppercase. |
| `String trim()` | Removes whitespaces from both ends of the string. |
| `String replace(char oldChar, char newChar)` | Replaces all occurrences of `oldChar` with `newChar`. |
| `boolean contains(CharSequence sequence)` | Returns `true` if the string contains the given sequence. |
| `char[] toCharArray()` | Converts the string into a character array. |
| `boolean startsWith(String prefix)` | Returns `true` if the string starts with the given prefix. |

---
