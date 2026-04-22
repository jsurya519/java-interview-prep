Object class is the root of the java class hierarchy. Every class in Java either directly or indirectly extends Object. If a class does not extend any other class then it is a direct child class of Object, and if it extends another class, then it is indirectly derived. Therefore the Object class methods are available to all Java classes. It provides essential methods like toString(), equals(), hashCode(), clone(), etc..

![da23b408ead64a6ae23a7a84a66e4000.png](:/8e11c386762a45ceac2cf8fc4b0ddf66)

## 1. toString() method:
toString() provides a String representation of an object and is used to convert an object to a String.

```java
class Student{

    String name = "Vishnu";
    int age = 21;

    @Override
    public String toString(){

        return "Student{name='" + name + "', age=" + age + "}";
    }

    public static void main(String[] args) {
        Student s = new Student();

         // Calls overridden toString()
        System.out.println(s.toString());
    }
}
```


## 2. hashCode() Method
hashCode() method returns the hash value of an object (not its memory address). Used heavily in hash-based collections like HashMap, HashSet, etc.

```java
class Employee {
    int id;
    Employee(int id)
    {
        this.id=id;
    }
}
public class Main {
    public static void main(String[] args) {
        Employee e1= new Employee(100);
		System.out.println(e1.hashCode());

        Employee e2 = new Employee(101);
        System.out.println(e2.hashCode());

		e1.id=103;
		System.out.println(e1.hashCode());
    }
}
```

Object created → hash code generated (based on identity)

Object fields change → ❌ hash code stays the same


**Another example:**
We can override the hashCode method to write custom to find if the object is modified.

```java
class Employee{

    int id = 101;

    @Override
    public int hashCode(){

        return id * 31;   // Simple custom hash
    }

    public static void main(String[] args) {
        Employee e = new Employee();
        System.out.println(e.hashCode());
    }
}
```

## 3. equals(Object obj) Method

equals() method compares the given object with the current object. Default equals method just checks both of same memory address. It is recommended to override this method to define custom equality conditions.

```java
import java.io.*;

class Book
{
    int id;
    String title;

    Book(int id, String title)
    {
        this.id=id;
        this.title=title;
    }

    @Override
    public boolean equals(Object obj)
    {
        Book b = (Book) obj;
        return this.id == b.id && this.title.equals(b.title);
    }
}

public class Main {

    public static void main(String[] args) {

        Book b1 = new Book(1,"science" );
        Book b2 = new Book(1,"science" );
        System.out.println(b1.equals(b2));
    }
}


```

## 4. getClass() method

getClass() method returns the class object of "this" object and is used to get the actual runtime class of the object.
```java
public class Geeks{

    public static void main(String[] args)
    {
        Object o = new String("GeeksForGeeks");
        Class c = o.getClass();
        System.out.println("Class of Object o is: "
                           + c.getName());
    }
}
```

## 5. finalize() method
The finalize() method in Java is called by the Garbage Collector just before an object is destroyed. It allows the object to perform a clean-up activity. Clean-up activity means closing the resources associated with that object, like Database Connection, Network Connection or we can say resource de-allocation. Called only once per object by the Garbage Collector.

What is Finalization in Java?
Just before destroying any object, the garbage collector always calls the finalize() method to perform clean-up activities on that object. This process is known as Finalization in Java. The Garbage collector calls the finalize() method only once on any object.

```java
public class Geeks {
    public static void main(String[] args) {

        Geeks t = new Geeks();
        System.out.println(t.hashCode());

        t = null;

        // calling garbage collector
        System.gc();

        System.out.println("end");
    }

    @Override protected void finalize()
    {
        System.out.println("finalize method called");
    }
}
```


## 6. clone() method
clone() method creates and returns a new object that is a copy of the current object.

```java
class Student implements Cloneable{

    int id = 1;
    String name = "Vishnu";

    @Override
    public Object clone() throws CloneNotSupportedException{

        return super.clone();  // shallow copy
    }

    public static void main(String[] args) throws Exception{

        Student s1 = new Student();
        Student s2 = (Student) s1.clone();

        System.out.println(s1.name); // Vishnu
        System.out.println(s2.name); // Vishnu
    }
}
```

clone() creates a copy of the current object (shallow copy by default). Class must implement Cloneable or else it throws CloneNotSupportedException.
