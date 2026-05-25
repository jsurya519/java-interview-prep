## Map Interface:
1. It represents a collection of key-value pairs, where Keys should be unique, but values can be duplicated.
2. HashMap and LinkedHashMap allow one null key, and TreeMap does NOT allow null keys
3. It provides efficient retrieval, insertion, and deletion operations based on keys.

<img width="800" height="400" alt="image" src="https://github.com/user-attachments/assets/d6034568-f581-4621-86ce-a054a5f11b33" />


## Implemented Classes of Map Interface:
**1. HashMap:** Stores key-value pairs using hashing for fast access, insertion, and deletion.

**2. LinkedHashMap:** Similar to HashMap but maintains the insertion order of key-value pairs.

**3. TreeMap:** Stores key-value pairs in sorted order using natural ordering or a custom comparator.

**4. Hashtable:** A synchronized Map implementation that doesn’t allow null keys or values.

```java
public class Dummy {

    public static void main(String[] args){
        // Create HashMap
        HashMap<Integer, String> students = new HashMap<>();

        // put(key, value)
        students.put(101, "CSE");
        students.put(102, "PRD");
        students.put(103, "IT");

        System.out.println("Initial Map: " + students);

        // get(key)
        String name = students.get(101);
        System.out.println("\nget(101): " + name);

        // remove(key)
        students.remove(102);
        System.out.println("\nAfter remove(102): " + students);

        // containsKey(key)
        System.out.println("\ncontainsKey(103): "
                + students.containsKey(103));

        // containsValue(value)
        System.out.println("containsValue(\"Priya\"): "
                + students.containsValue("IT"));

        // putIfAbsent(key, value)
        students.putIfAbsent(103, "Kiran"); // won't replace
        students.putIfAbsent(104, "Kiran"); // added

        System.out.println("\nAfter putIfAbsent:");
        System.out.println(students);

        // getOrDefault(key, defaultValue)
        String result1 = students.getOrDefault(105, "Not Found");
        String result2 = students.getOrDefault(101, "Not Found");

        System.out.println("\ngetOrDefault(105): " + result1);
        System.out.println("getOrDefault(101): " + result2);

        // keySet()
        System.out.println("\nAll Keys:");
        System.out.println(students.keySet());

        // values()
        System.out.println("\nAll Values:");
        System.out.println(students.values());

        // entrySet()
        System.out.println("\nEntry Set:");

        for (Map.Entry<Integer, String> entry : students.entrySet()) {

            System.out.println(
                    "Key = " + entry.getKey()
                            + ", Value = " + entry.getValue()
            );
        }
    }
}
// Output:
// Initial Map: {101=CSE, 102=PRD, 103=IT}

// get(101): CSE

// After remove(102): {101=CSE, 103=IT}

// containsKey(103): true
// containsValue("Priya"): true

// After putIfAbsent:
// {101=CSE, 103=IT, 104=Kiran}

// getOrDefault(105): Not Found
// getOrDefault(101): CSE

// All Keys:
// [101, 103, 104]

// All Values:
// [CSE, IT, Kiran]

// Entry Set:
// Key = 101, Value = CSE
// Key = 103, Value = IT
// Key = 104, Value = Kiran
```
