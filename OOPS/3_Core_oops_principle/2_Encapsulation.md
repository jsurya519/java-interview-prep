## Encapsulation:
Encapsulation in Java is an object-oriented principle that binds data and methods into a single unit, typically a class. It restricts direct access to data by hiding implementation details.

## How to achieve Encapsulation:
1. private data members
2. public getter and setter methods

## Key Rules:
**1. Declare data as private:** Hide the class data so it cannot be accessed directly from outside the class.
**2. Use getters and setters:** Keep variables private and provide public getter and setter methods for controlled access and safe modification, often with validation.
**3. Apply proper access modifiers:** Use private for data hiding and public for methods that provide access

```java
class Programmer {

    private String name;

    // Getter method used to get the data
    public String getName() { return name; }

    // Setter method is used to set or modify the data
    public void setName(String name) {

        this.name = name;
    }
}

public class Geeks {

    public static void main(String[] args){

        Programmer p = new Programmer();
        p.setName("Geek");
        System.out.println("Name=> " + p.getName());
    }
}
// Output:
// Name=> Geek
```