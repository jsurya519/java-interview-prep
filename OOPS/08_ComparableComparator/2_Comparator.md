## Comparator Interface:
1. It is used to define custom sorting logic for objects.
2. We need multiple sorting strategies for a class.
3. We want to keep sorting logic separate from the class definition.
**4. Comparator is a functional interface as it contains only one abstract method compare().**

```java
@FunctionalInterface
public interface Comparator<T> {
	int compare(T o1, T o2);
}
```

**Implementation before java 8**

```java
import java.util.*;

class Employee
{
    int id;
    int salary;
    private String name;

    Employee(int id, int salary, String name)
    {
        this.id=id;
        this.salary=salary;
        this.name=name;
    }

    static void print(List<Employee> l)
    {
        for(Employee e:l)
        {
            System.out.println(e.id + " " + e.salary + " "+e.name);
        }
    }
}

class EmployeeIdSorting implements Comparator<Employee>
{
    @Override
    public int compare(Employee e1, Employee e2)
    {
        return e1.id-e2.id;
    }
}

class EmployeeSalarySorting implements Comparator<Employee>
{
    @Override
    public int compare(Employee e1, Employee e2)
    {
        return e1.salary-e2.salary;
    }
}

public class Dummy {
    public static void main(String[] args) {

        List<Employee> list = new ArrayList<>();
        list.add(new Employee(50, 3000, "abc"));
        list.add(new Employee(20, 4000, "rave"));
        list.add(new Employee(100, 1000, "prav"));

        Comparator<Employee> c = new EmployeeIdSorting();
        Collections.sort(list, c);
        Employee.print(list);

        System.out.println("-------------");
        c =  new EmployeeSalarySorting();
        Collections.sort(list, c);
        Employee.print(list);

    }
}
// Output:
// 20 4000 rave
// 50 3000 abc
// 100 1000 prav
// -------------
// 100 1000 prav
// 50 3000 abc
// 20 4000 rave
```


**Implementation using Anonymous class:**

```java
package structuralPatterns;

import java.util.*;

class Employee
{
    int id;
    int salary;
    private String name;

    Employee(int id, int salary, String name)
    {
        this.id=id;
        this.salary=salary;
        this.name=name;
    }

    static void print(List<Employee> l)
    {
        for(Employee e:l)
        {
            System.out.println(e.id + " " + e.salary + " "+e.name);
        }
    }
}

public class Dummy {
    public static void main(String[] args) {

        List<Employee> list = new ArrayList<>();
        list.add(new Employee(50, 3000, "abc"));
        list.add(new Employee(20, 4000, "rave"));
        list.add(new Employee(100, 1000, "prav"));

        Comparator<Employee> employeeIdComparator = new Comparator<Employee>()
        {
          @Override
          public int compare(Employee e1, Employee e2)
          {
              return e1.id-e2.id;
          }
        };
        Collections.sort(list, employeeIdComparator);
        Employee.print(list);

        System.out.println("-------------");
        Comparator<Employee> employeeSalaryComparator = new Comparator<Employee>()
        {
            @Override
            public int compare(Employee e1, Employee e2)
            {
                return e1.salary-e2.salary;
            }
        };
        Collections.sort(list, employeeSalaryComparator);
        Employee.print(list);

    }
}
// Output:
// 20 4000 rave
// 50 3000 abc
// 100 1000 prav
// -------------
// 100 1000 prav
// 50 3000 abc
// 20 4000 rave
```

**Implementation using lambda expression:**

```java
package structuralPatterns;

import java.util.*;

class Employee
{
    int id;
    int salary;
    private String name;

    Employee(int id, int salary, String name)
    {
        this.id=id;
        this.salary=salary;
        this.name=name;
    }

    static void print(List<Employee> l)
    {
        for(Employee e:l)
        {
            System.out.println(e.id + " " + e.salary + " "+e.name);
        }
    }
}

public class Dummy {
    public static void main(String[] args) {

        List<Employee> list = new ArrayList<>();
        list.add(new Employee(50, 3000, "abc"));
        list.add(new Employee(20, 4000, "rave"));
        list.add(new Employee(100, 1000, "prav"));

        Comparator<Employee> employeeIdComparator = (Employee e1, Employee e2) -> {
            return e1.id-e2.id;
        };
        Collections.sort(list, employeeIdComparator);
        Employee.print(list);

        System.out.println("-------------");
        Comparator<Employee> employeeSalaryComparator = (Employee e1, Employee e2) -> {
            return e1.salary-e2.salary;
        };
        Collections.sort(list, employeeSalaryComparator);
        Employee.print(list);

    }
}
// Output:
// 20 4000 rave
// 50 3000 abc
// 100 1000 prav
// -------------
// 100 1000 prav
// 50 3000 abc
// 20 4000 rave
```

More concise version:

```java
package structuralPatterns;

import java.util.*;

class Employee
{
    int id;
    int salary;
    private String name;

    Employee(int id, int salary, String name)
    {
        this.id=id;
        this.salary=salary;
        this.name=name;
    }

    static void print(List<Employee> l)
    {
        for(Employee e:l)
        {
            System.out.println(e.id + " " + e.salary + " "+e.name);
        }
    }
}

public class Dummy {
    public static void main(String[] args) {

        List<Employee> list = new ArrayList<>();
        list.add(new Employee(50, 3000, "abc"));
        list.add(new Employee(20, 4000, "rave"));
        list.add(new Employee(100, 1000, "prav"));


        Collections.sort(list, (e1,e2)->e1.id-e2.id);
        Employee.print(list);

        System.out.println("-------------");
        Collections.sort(list, (e1,e2)->e1.salary-e2.salary);
        Employee.print(list);

    }
}
// Output:
// 20 4000 rave
// 50 3000 abc
// 100 1000 prav
// -------------
// 100 1000 prav
// 50 3000 abc
// 20 4000 rave
```