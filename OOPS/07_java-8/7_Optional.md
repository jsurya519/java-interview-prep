## Optional:
1. Optional class is used handle null values gracefully.
2. It helps to avoid Null Pointer Exception (NPE).

## Creating Optional:
```java
//using of ---  direct null values not allowed.
Optional<String> op1 = Optional.of("CSE");

// Using ofNullable --- null values also allowed.
Optional<String> op2 = Optional.ofNullable(null);

// Using empty   ---- It represents null.
Optional<String> op3 = Optional.empty();
```

## Get Optional
```java
Optional<String> op1 = Optional.empty();
op1.get();

// Output:
// Exception in thread "main" java.util.NoSuchElementException: No value present
```

```java
Optional<String> op1 = Optional.ofNullable(null);
op1.get();
// Output:
// Exception in thread "main" java.util.NoSuchElementException: No value present****
```


```java
Optional<String> op1 = Optional.ofNullable("CSE");
if(op1.isPresent())
	System.out.println(op1.get());

// Output:
// CSE
```

```java
Optional<String> op1 = Optional.ofNullable(null);

if(op1.isPresent())
	System.out.println(op1.get());
else
    System.out.println("null value");

// Output:
//null value
```


---

**Advanced:**

```java
Optional<String> op1 = Optional.ofNullable("CSE");
System.out.println(op1.orElse("default"));

// Output:
// CSE
```

```java
Optional<String> op1 = Optional.ofNullable(null);
System.out.println(op1.orElse("default"));

// Output:
// default
```


```java
Optional<String> op1 = Optional.ofNullable("CSE");
System.out.println(op1.orElseGet(() -> "default"));

// Output:
// CSE
```

```java
Optional<String> op1 = Optional.ofNullable("CSE");
System.out.println(op1.orElseThrow(() -> new RuntimeException("Our exception")));
```

```java
Optional<String> op1 = Optional.ofNullable("CSE");
op1.ifPresent(x-> System.out.println(x));
```

