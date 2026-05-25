## Method Reference:
1. In method reference, we don't need to write method definition. It helps to refer to an existing method that matches with return type and parameter list.
2. This works only with functional interface, as an alternative to lambda expression.

```java
@FunctionalInterface
interface Employee
{
    void getSalary();
}

class Student
{
    void fun()
    {
        System.out.println("Hello world");
    }
}

public class Dummy {
    public static void main(String[] args) {

//        Using Lambda expressions
//        Employee e = () -> System.out.println("Hello world");
//        e.getSalary();

        Student s = new Student();
        Employee e = s::fun;
        e.getSalary();
    }
}
```


```java
import java.util.ArrayList;
import java.util.List;

public class Dummy {
    // Method
    public static void print(String s){
        System.out.println(s);
    }

    public static void main(String[] args){

        List<String> arr = new ArrayList<>();
        arr.add("hello");
        arr.add("world");
        arr.add("how");
        arr.add("are");
        arr.add("you");

        arr.forEach(Dummy::print);
    }
}
```

---

## Types of Method Reference:

There are four types of method reference
1. Reference to a static method
2. Reference to an instance method of particular object
3. Reference to an instance method of arbitrary object
4. Reference to constructor

## 1. Reference to a static method
```java
import java.util.*;

class MathUtil{
    static void square(int n) {
        System.out.println(n * n);
    }
}

class GFG{
    public static void main(String[] args) {

        List<Integer> list = Arrays.asList(2, 3, 4);
        list.forEach(MathUtil::square);
    }
}
```

## 2. Reference to an Instance Method of a Particular Object
```java
import java.util.*;

class Printer{
    void print(String msg) {
        System.out.println(msg);
    }
}

class GFG{
    public static void main(String[] args) {

        Printer printer = new Printer();
        List<String> data = Arrays.asList("Java", "Spring", "Boot");

        data.forEach(printer::print);
    }
}
```

```java
import java.util.ArrayList;
import java.util.List;

public class Dummy {

    public static void main(String[] args){
        List<String> s = new ArrayList<>();
        s.add("abc");
        s.add("def");
        s.add("ggj");

        s.forEach(x-> System.out.println(x));

        s.forEach(System.out::println);
    }
}
```
## 3. Reference to an instance method of arbitrary object
This reference works for a instance method without explicitly creating instance for the class.

**Important rule:**
```java
ClassName::instanceMethod
```

Converts the above into
```java
(firstParam) -> firstParam.instanceMethod();

(firstParam, remainingParams) ->
    firstParam.instanceMethod(remainingParams);
```

```java
import java.util.function.Consumer;


class Employee
{
    void fun()
    {
        System.out.println("Hello world");
    }
}

public class Dummy {

    public static void main(String[] args){

        Consumer<Employee> c = (Employee e) -> e.fun();
        c.accept(new Employee());

        Consumer<Employee> cc = Employee::fun;
        cc.accept(new Employee());
    }
}
```


**Another example:**

```java
class Employee {

    String name;

    Employee(String name) {
        this.name = name;
    }

    // Instance method
    public void display(String msg) {
        System.out.println(msg + " " + name);
    }
}

@FunctionalInterface
interface MyInterface {

    void execute(Employee e, String msg);
}

public class Test {

    public static void main(String[] args) {

        // Arbitrary object instance method reference
        MyInterface ref = Employee::display;

        Employee e1 = new Employee("John");

        ref.execute(e1, "Hello");
    }
}
```

**Another example with UnaryOperator:**

```java

import java.util.function.UnaryOperator;

class Student
{
    int id;
    String name;

    Student(int id, String name)
    {
        this.id=id;
        this.name=name;
    }

    Student changeName()
    {
        this.name = "DUMMY";
        return this;
    }
}

public class Dummy {

    public static void main(String[] args){

        Student s = new Student(1, "jay");
        System.out.println(s.id  + " " + s.name);

        UnaryOperator<Student> tt = Student::changeName;
        tt.apply(s);
        System.out.println(s.id  + " " + s.name);
    }
}
```