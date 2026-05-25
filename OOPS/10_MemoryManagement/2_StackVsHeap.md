## Java Stack vs Heap Memory Allocation
**Stack Memory:** Stores primitive local variables, method call information, and references to objects during program execution.
**Heap Memory:** Stores actual objects and dynamic data allocated at runtime. Objects created with new are placed here, and this memory is managed by the Garbage Collector.

```java
class Employee {
    int id;        // Primitive stored inside the object in Heap
    String name;   // Reference points to String object in Heap
    double salary; // Primitive stored inside the object in Heap

    public Employee(int id, String name, double salary) {
        this.id = id;
        this.name = name;
        this.salary = salary;
    }

    public void display() {
        System.out.println("Employee ID: " + id);
        System.out.println("Name: " + name);
        System.out.println("Salary: " + salary);
    }
}

public class Main {
    // Reference variable 'emp' stored in Stack
    public static void display(Employee emp) {
        emp.display(); // Accesses Employee object in Heap
    }

    public static void main(String[] args) {
        // 'emp1' and 'emp2' references in Stack, objects in Heap
        Employee emp1 = new Employee(101, "Maddy", 50000.0);
        Employee emp2 = new Employee(102, "Maddy", 60000.0);

        display(emp1);
        display(emp2);
    }
}
```

**Memory representation for the above example:**
![98b6006738fa511c3e6dba1cd48ef833.png](:/af1529bf0e094f37b6ac7be58787f9a5)