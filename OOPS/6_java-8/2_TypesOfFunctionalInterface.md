## Types of Functional Interface:
There are mainly four functional interface types.  These are widely used in Stream API, collections and lambda-based operations.

![73e9d7dc8e58209a4d4de9285b47abc9.png](:/b0d63705b5a6459996ba3e06e21cc162)

## 1. Consumer
Consumer interface of the functional interface is the one that accepts only one argument. It is used for performing an action, such as printing or logging.

```java
@FunctionalInterface
public interface Consumer<T> {

    void accept(T t);
}
```

There are also functional variants of the Consumer DoubleConsumer, IntConsumer and LongConsumer.

## 2. Predicate:
Predicate interface represents a boolean-valued function of one argument. It is commonly used for filtering operations in streams. There are also functional variants of the Predicate IntPredicate, DoublePredicate and LongPredicate.

```java
@FunctionalInterface
public interface Predicate<T> {
    boolean test(T t);
}
```

## 3. Function:
The function interface takes one argument and returns a result. It is commonly used for transforming data. Several variations exist:

1. Bi-Function: Takes two arguments and returns a result.
2. Unary Operator: Input and output are of the same type.
3. Binary Operator: Like BiFunction but with same input/output type.

**1. BiFunction:**
```java
@FunctionalInterface
public interface BiFunction<T, U, R> {

    R apply(T t, U u);
}
```

**2. Unary Operator:**
```java
@FunctionalInterface
public interface UnaryOperator<T>
        extends Function<T, T> {
}
```

**3. Binary Operator:**
```java
@FunctionalInterface
public interface BinaryOperator<T>
        extends BiFunction<T, T, T> {
}
```

---
## 4. Supplier:
Supplier functional interface is also a type of functional interface that does not take any input or argument and yet returns a single output. The different extensions of the Supplier functional interface hold many other suppliers functions like BooleanSupplier, DoubleSupplier, LongSupplier and IntSupplier.

```java
@FunctionalInterface
public interface Supplier<T> {

    T get();
}
```

