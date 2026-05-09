## Lambda Expression:
1. Lambda expressions implement a functional interface (An interface with only one abstract function)
2. Enable passing code as data (method arguments).
3. Lambda expressions can access only final or effectively final variables from the enclosing scope.
4. Lambdas cannot throw checked exceptions unless the functional interface declares them.
5. Allow defining behavior without creating separate classes.

```java
interface Add{

    int addition(int a, int b);
}

public class GFG{

    public static void main(String[] args){

        // Lambda expression to add two numbers
        Add add = (a, b) -> { return a + b };

        int result = add.addition(10, 20);
        System.out.println("Sum: " + result);
    }
}
// Output:
// Sum: 30
```

```java
interface FuncInterface{

    void abstractFun(int x);
    default void normalFun(){
        System.out.println("Hello");
        }
}

public class GFG{

    public static void main(String[] args){

        FuncInterface fobj = (int x) -> System.out.println(2 * x);
        fobj.abstractFun(5);
    }
}
// Output:
// 10
```

```java
@FunctionalInterface
interface Functional {
    int operation(int a, int b);
}

public class Test {

    public static void main(String[] args) {

        // Using lambda expressions to define the operations
        Functional add = (a, b) -> a + b;
        Functional multiply = (a, b) -> a * b;

        // Using the operations
        System.out.println(add.operation(6, 3));
        System.out.println(multiply.operation(4, 5));
    }
}
// Output:
// 9
// 20
```

**Note:**
Lambda expressions are just like functions and they accept parameters just like functions.

---
## Validity Check: Example Lambda Expressions

| Expression | Validity | Reason |
|---|---|---|
| `() -> {}` | Valid | No parameters, empty body |
| `() -> "geeksforgeeks"` | Valid | Single expression returns value |
| `() -> { return "geeksforgeeks"; }` | Valid | Uses braces with return keyword |
| `(Integer i) -> { return "geeksforgeeks" + i; }` | Valid | Correct syntax with typed parameter |
| `(String s) -> { return "geeksforgeeks"; }` | Valid | Parameter unused but valid |
| `() -> { return "Hello" }` | Invalid | Missing semicolon after return statement |
| `x -> { return x + 1; }` | Invalid | Invalid if type inference not possible |
| `(int x, y) -> x + y` | Invalid | If one parameter has type, all must |


| Parameters              | Parentheses |
| ----------------------- | ----------- |
| No parameters           | Mandatory   |
| One parameter           | Optional    |
| One parameter with type | Mandatory   |
| Multiple parameters     | Mandatory   |


---
