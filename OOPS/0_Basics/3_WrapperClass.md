## Wrapper Class:

In Java, wrapper classes allow primitive data types to be represented as objects. This enables primitives to be used in object-oriented features such as collections, generics, and APIs that require objects.

1. Each wrapper class encapsulates a corresponding primitive value inside an object (e.g., Integer for int, Double for double).
2. Java provides wrapper classes for all eight primitive data types to support object-based operations.

```java
class Geeks {
    public static void main(String[] args) {

        int b = 357;

        // Autoboxing: primitive int -> Integer object
        Integer a = b;

        System.out.println("The primitive int b is: " + b);
        System.out.println("The Integer object a is: " + a);
    }
}
// Output:
// The primitive int b is: 357
// The Integer object a is: 357
```


## Why wrapper class are needed:
1. Java collections (ArrayList, HashMap, etc.) store only objects, not primitives.
2. Wrapper objects allow primitives to be used in object-oriented features like methods, synchronization, and serialization.
3. Objects support null values, while primitives do not.
4. Wrapper classes provide utility methods such as compareTo(), equals(), and toString().

## Autoboxing and Unboxing

**1. Autoboxing**
The automatic conversion of primitive types to the object of their corresponding wrapper classes is known as autoboxing. For example: conversion of int to Integer, long to Long, double to Double, etc.

```java
import java.util.ArrayList;

class Geeks {
    public static void main(String[] args) {
        char ch = 'a';

        // Autoboxing: char -> Character
        Character c = ch;

        ArrayList<Integer> list = new ArrayList<>();
        // Autoboxing: int -> Integer
        list.add(25);
        System.out.println(list.get(0));
    }
}

// output:
// 25
```

**2. Unboxing**
Unboxing is the automatic conversion of a wrapper class object back into its corresponding primitive type.

```java
import java.util.ArrayList;

class Geeks {
    public static void main(String[] args) {

        Character ch = 'a';
        // Unboxing: Character -> char
        char c = ch;

        ArrayList<Integer> list = new ArrayList<>();
        list.add(24);
        // Unboxing: Integer -> int
        int num = list.get(0);

        System.out.println(num);
    }
}

// Output:
// 24
```

**Java Wrapper Classes Example**

```java
class Geeks {
    public static void main(String[] args) {

        byte b = 1;
        Byte byteObj = Byte.valueOf(b);

        int i = 10;
        Integer intObj = Integer.valueOf(i);

        float f = 18.6f;
        Float floatObj = Float.valueOf(f);

        double d = 250.5;
        Double doubleObj = Double.valueOf(d);

        char c = 'a';
        Character charObj = c; // autoboxing

        System.out.println("Wrapper Objects:");
        System.out.println(byteObj);
        System.out.println(intObj);
        System.out.println(floatObj);
        System.out.println(doubleObj);
        System.out.println(charObj);

        // Unboxing
        byte bv = byteObj;
        int iv = intObj;
        float fv = floatObj;
        double dv = doubleObj;
        char cv = charObj;

        System.out.println("\nUnwrapped values:");
        System.out.println(bv);
        System.out.println(iv);
        System.out.println(fv);
        System.out.println(dv);
        System.out.println(cv);
    }
}
```

---

## Primitive Data Types & Wrapper Classes

| Primitive Data Type | Wrapper Class |
|--------------------|--------------|
| byte               | Byte         |
| short              | Short        |
| int                | Integer      |
| long               | Long         |
| float              | Float        |
| double             | Double       |
| char               | Character    |
| boolean            | Boolean      |