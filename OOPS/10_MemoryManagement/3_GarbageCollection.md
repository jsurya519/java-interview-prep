## Garbage Collection
1. Garbage collection is an automatic process that removes unused objects from heap.
2. It identifies which objects are still in use (referenced) and which are not in use (unreferenced).
3. It removes the objects that are unreachable (no longer referenced).
4. The programmer does not need to mark objects to be deleted explicitly. Garbage collection is implemented within the JVM.

## Java heap is divided into generations
**1.Young Generation:** In this new objects are allocated.
**2.Old Generation:** In this long-lived objects are stored.

## Types of garbage collection:
1. **Minor GC**: Cleans up short-lived objects from the Young Generation. It happens frequently and is usually fast.
2. **Major GC (Full GC)**: Cleans up objects from the Old Generation (and sometimes entire heap). It happens less often and is slower.

## Key Concepts on Garbage Collection
**1. Unreachable Objects**
An object becomes unreachable if it does not contain any reference to it.
```java
Integer i = new Integer(4);
// the new Integer object is reachable  via the reference in 'i'
i = null;
// the Integer object is no longer reachable.
```

**2. Making Objects Eligible for GC**
An object is said to be eligible for garbage collection if it is unreachable. After i = null, integer object 4 in the heap area is suitable for garbage collection in the above example.

**How to Make an Object Eligible for Garbage Collection?**
Even though the programmer is not responsible for destroying useless objects but it is highly recommended to make an object unreachable(thus eligible for GC) if it is no longer required. There are generally four ways to make an object eligible for garbage collection.

1. Nullifying the reference variable (obj = null).
2. Re-assigning the reference variable (obj = new Object()).
3. An object created inside the method (eligible after method execution).

**3. Requesting Garbage Collection**
1. Once an object is eligible for garbage collection, it may not be destroyed immediately.The garbage collector runs at the JVM's discretion and you cannot predict when it will occur.
2. We can also request JVM to run Garbage Collector. There are two ways to do it.
	1. System.gc()
	2. Runtime.getRuntime().gc():
**Using System.gc():** This static method requests the JVM to perform garbage collection.
**Using Runtime.getRuntime().gc():** This method also requests garbage collection through the Runtime class.

```java
System.gc();
// OR
Runtime.getRuntime().gc();
```

**4. The finalize() Method (Deprecated in Java 9+)**
Before destroying an object, the garbage collector calls the finalize() method to perform cleanup activities. The method is defined in the Object class as follows:
```java
@Override
protected void finalize() throws Throwable {
System.out.println("GC cleaning up...");
}
```

**Note:**
1. finalize() method is deprecated since Java 9 because it is unpredictable and can cause performance issues.
2. The garbage collector calls finalize() at most once per object.
