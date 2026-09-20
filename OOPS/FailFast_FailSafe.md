# Fail-Fast vs Fail-Safe Iterators in Java

## 1. Fail-Fast Iterator

A **fail-fast iterator** detects structural modification of a collection while it is being iterated and throws `ConcurrentModificationException`.

Common examples:

* `ArrayList`
* `HashSet`
* `HashMap`
* `LinkedList`

### Example

```java
import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;

public class FailFastExample {

    public static void main(String[] args) {

        List<Integer> numbers = new ArrayList<>();

        numbers.add(10);
        numbers.add(20);
        numbers.add(30);

        Iterator<Integer> iterator = numbers.iterator();

        while (iterator.hasNext()) {

            Integer number = iterator.next();

            if (number == 20) {
                numbers.remove(number); // ❌ Structural modification
            }
        }
    }
}
```

This can result in:

```text
java.util.ConcurrentModificationException
```

### Why does this happen?

`ArrayList` maintains an internal modification count.

Conceptually:

```text
ArrayList
    |
    |-- modCount = number of structural modifications
    |
Iterator
    |
    |-- expectedModCount
```

When the iterator is created:

```text
modCount = 3
expectedModCount = 3
```

If the collection is structurally modified:

```text
numbers.remove(20);

modCount = 4
expectedModCount = 3
```

When the iterator subsequently checks the collection:

```text
modCount != expectedModCount
        |
        ↓
ConcurrentModificationException
```

This is called **fail-fast** because the iterator tries to detect the problem quickly rather than continuing with potentially inconsistent iteration.

---

## 2. Correct Way to Remove While Iterating

If we need to remove an element while using an iterator, use the iterator's own `remove()` method.

```java
import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;

public class IteratorRemoveExample {

    public static void main(String[] args) {

        List<Integer> numbers = new ArrayList<>();

        numbers.add(10);
        numbers.add(20);
        numbers.add(30);

        Iterator<Integer> iterator = numbers.iterator();

        while (iterator.hasNext()) {

            Integer number = iterator.next();

            if (number == 20) {
                iterator.remove(); // ✅ Safe
            }
        }

        System.out.println(numbers);
    }
}
```

Output:

```text
[10, 30]
```

The important difference:

```java
numbers.remove(20);    // ❌ Direct modification while iterating

iterator.remove();     // ✅ Iterator knows about the modification
```

---

# 3. What Does "Fail-Safe" Mean?

**Fail-safe** is a commonly used interview term, but it is not an official Java collection category.

It generally refers to iterators that can continue iteration even if the underlying collection is modified.

Examples include:

* `CopyOnWriteArrayList` → snapshot-style iteration
* `ConcurrentHashMap` → weakly consistent iteration

---

# 4. CopyOnWriteArrayList Example

```java
import java.util.List;
import java.util.concurrent.CopyOnWriteArrayList;

public class FailSafeExample {

    public static void main(String[] args) {

        List<Integer> numbers =
                new CopyOnWriteArrayList<>();

        numbers.add(10);
        numbers.add(20);
        numbers.add(30);

        for (Integer number : numbers) {

            System.out.println(number);

            if (number == 20) {
                numbers.remove(Integer.valueOf(20));
            }
        }

        System.out.println(numbers);
    }
}
```

The iteration does not throw:

```text
ConcurrentModificationException
```

The iterator works over a snapshot of the array that existed when the iterator was created.

Conceptually:

```text
Original array
[10] [20] [30]
      ↑
      Iterator created
      |
      └── Iterator sees this snapshot

Then collection changes:

[10] [30]

Iterator can still continue using its snapshot.
```

Therefore, the iterator may see:

```text
10
20
30
```

even though `20` was removed from the current collection during iteration.

---

# 5. ConcurrentHashMap Example

`ConcurrentHashMap` provides a different behavior.

Its iterators are generally described as **weakly consistent** rather than fail-safe.

```java
import java.util.Map;
import java.util.concurrent.ConcurrentHashMap;

public class ConcurrentHashMapExample {

    public static void main(String[] args) {

        Map<Integer, String> users =
                new ConcurrentHashMap<>();

        users.put(1, "Jaya");
        users.put(2, "Surya");
        users.put(3, "Java");

        for (Map.Entry<Integer, String> entry : users.entrySet()) {

            System.out.println(entry);

            users.put(4, "Spring");
        }
    }
}
```

The iterator does not throw `ConcurrentModificationException`.

However, you should **not assume a fixed snapshot** like `CopyOnWriteArrayList`.

The iterator is weakly consistent:

* It does not throw `ConcurrentModificationException`.
* It may reflect some modifications made after iteration began.
* It does not necessarily reflect every modification.
* It does not provide a fully consistent snapshot.

---

# 6. Fail-Fast vs "Fail-Safe"

| Feature                           | Fail-Fast                                  | "Fail-Safe" / Concurrent Iteration                          |
| --------------------------------- | ------------------------------------------ | ----------------------------------------------------------- |
| Common examples                   | `ArrayList`, `HashMap`, `HashSet`          | `CopyOnWriteArrayList`, `ConcurrentHashMap`                 |
| Concurrent modification           | Detected                                   | Supported with collection-specific semantics                |
| `ConcurrentModificationException` | May throw                                  | Generally does not throw because of concurrent modification |
| Iterator behavior                 | Detects unexpected structural modification | Snapshot or weakly consistent behavior                      |
| Main idea                         | Detect modification quickly                | Allow iteration to proceed                                  |

---

# 7. Very Important: Fail-Fast Is Not Guaranteed Thread-Safety

This is an important interview point.

A fail-fast iterator does **not** mean:

> "The collection is thread-safe."

For example:

```java
List<Integer> list = new ArrayList<>();
```

`ArrayList` is not thread-safe.

Fail-fast behavior is mainly a **bug-detection mechanism**.

Also, Java's documentation describes fail-fast behavior as **best-effort**. Therefore, we should not write program logic that depends on `ConcurrentModificationException` always being thrown.

---

# 8. Structural Modification

When discussing fail-fast behavior, remember the term:

> **Structural modification**

Examples:

```java
list.add(10);
list.remove(10);
list.clear();
```

These change the structure/size of the collection.

By contrast:

```java
list.set(0, 100);
```

replaces an existing element and does not change the size of an `ArrayList`, so it is not considered a structural modification for the purpose of `ArrayList`'s iterator.

---

# 9. Easy Mental Model

### ArrayList

```text
ArrayList
    ↓
Iterator created
    ↓
Iterator expects collection
to remain structurally unchanged
    ↓
Collection modified directly
    ↓
Iterator detects modification
    ↓
ConcurrentModificationException
```

### CopyOnWriteArrayList

```text
CopyOnWriteArrayList
    ↓
Iterator created
    ↓
Iterator works with a snapshot
    ↓
Collection modified
    ↓
Iterator continues
```

### ConcurrentHashMap

```text
ConcurrentHashMap
    ↓
Iterator created
    ↓
Weakly consistent iterator
    ↓
Collection may change
    ↓
Iterator continues
    ↓
May or may not reflect
some modifications
```

---

# 10. Interview Answer

If the interviewer asks:

**"What is fail-fast vs fail-safe?"**

A good answer is:

> **"A fail-fast iterator detects unexpected structural modification of a collection during iteration and typically throws `ConcurrentModificationException`. Examples include `ArrayList` and `HashMap`.**
>
> **The term fail-safe is commonly used in interviews for iterators that don't fail with `ConcurrentModificationException` when the collection changes, but Java's more precise terminology depends on the collection. For example, `CopyOnWriteArrayList` iterates over a snapshot, while `ConcurrentHashMap` provides weakly consistent iteration."**

Then give the example:

```java
List<Integer> list = new ArrayList<>();

for (Integer value : list) {
    list.remove(value); // ❌ May cause ConcurrentModificationException
}
```

versus:

```java
List<Integer> list =
        new CopyOnWriteArrayList<>();

for (Integer value : list) {
    list.remove(value); // ✅ Iterator does not fail
}
```

---

# 11. Key Points to Remember

```text
Fail-Fast
    ↓
Unexpected structural modification
    ↓
ConcurrentModificationException
    ↓
ArrayList / HashMap / HashSet
```

```text
CopyOnWriteArrayList
    ↓
Snapshot iterator
    ↓
Modification doesn't affect current iteration
```

```text
ConcurrentHashMap
    ↓
Weakly consistent iterator
    ↓
Can continue while map changes
    ↓
May reflect some changes
```

### One-line memory trick

> **Fail-fast = "You changed the collection while I'm iterating → I'll complain."**

> **CopyOnWriteArrayList = "I'll iterate over my snapshot."**

> **ConcurrentHashMap = "I'll keep going, but my view may reflect some concurrent changes."**
