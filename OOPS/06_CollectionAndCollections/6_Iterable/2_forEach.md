## forEach in Iterable Interface:
1.  forEach() is generally used to iterate and perform an action on each element of a collection. It is commonly used for printing elements.
2. All concrete classes that directly or indirectly implement the Iterable interface inherit the forEach() method.

```java
public interface Iterable<T> {
    void forEach(Consumer<? super T> action);
}
```

**Using Anonymous class:**

```java
class Employee
{
    int id;
    int sal;
    String name;

    Employee(int id, int sal, String name)
    {
        this.id=id;
        this.sal=sal;
        this.name=name;
    }
}

public class Dummy {
    public static void main(String[] args) {

        List<Employee> list = new ArrayList<>();
        list.add(new Employee(1, 1000, "avc"));
        list.add(new Employee(2, 2000, "ccb"));
        list.add(new Employee(3, 3000, "fnvj"));


        list.forEach(new Consumer<Employee>()
        {
            @Override
            public void accept(Employee e)
            {
                System.out.println(e.id + " " + e.sal + " " + e.name);
            }
        });
    }
}
// Output:
// 1 1000 avc
// 2 2000 ccb
// 3 3000 fnvj
```

**Using lambda expression:**
```java

import java.util.ArrayList;
import java.util.List;

class Employee
{
    int id;
    int sal;
    String name;

    Employee(int id, int sal, String name)
    {
        this.id=id;
        this.sal=sal;
        this.name=name;
    }
}

public class Dummy {
    public static void main(String[] args) {

        List<Employee> list = new ArrayList<>();
        list.add(new Employee(1, 1000, "avc"));
        list.add(new Employee(2, 2000, "ccb"));
        list.add(new Employee(3, 3000, "fnvj"));


        list.forEach(e-> System.out.println(e.id + " "+e.sal+" "+e.name));
    }
}
```