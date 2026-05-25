## Constructor Reference:
Constructor reference are used to create Object with short form of lambda expression.

**1. Zero parameter:**
```java
class Student
{
    int id;
    String name;

    Student()
    {
        System.out.println("Constructor called");
        this.id = 10;
        this.name = "DUMMY";
    }

}

public class Dummy {

    public static void main(String[] args){

//      Using lambda expression:

        Supplier<Student> supplier = () -> {return new Student();};
        Student s =supplier.get();
        System.out.println(s.id + " " + s.name);


//        Using Constructor reference:
        Supplier<Student> ss = Student::new;
        Student st = ss.get();
        System.out.println(s.id + "  " + s.name);
    }
}
// Output:
// Constructor called
// 10 DUMMY
// Constructor called
// 10  DUMMY
```


**1. One parameter:**
```java
class Student
{
    int id;
    String name;

    Student(int id)
    {
        System.out.println("Constructor called");
        this.id = id;
        this.name = "DUMMY";
    }

}

public class Dummy {

    public static void main(String[] args){

//      Using lambda expression:

        Function<Integer, Student> function = (Integer id) -> {return new Student(id);};
        Student s = function.apply(50);
        System.out.println(s.id + " " + s.name);


//        Using Constructor reference:
        Function<Integer,Student> ss = Student::new;
        Student st = ss.apply(60);
        System.out.println(st.id + "  " + st.name);
    }
}
// Output:
// Constructor called
// 50 DUMMY
// Constructor called
// 60  DUMMY
```

**3. Two parameters:**

```java
class Student
{
    int id;
    String name;

    Student(int id, String name)
    {
        System.out.println("Constructor called");
        this.id = id;
        this.name = name;
    }

}

public class Dummy {

    public static void main(String[] args){

//      Using lambda expression:

        BiFunction<Integer, String, Student> function = (Integer id, String name) -> {return new Student(id, name);};
        Student s = function.apply(50, "jay");
        System.out.println(s.id + " " + s.name);


//        Using Constructor reference:
        BiFunction<Integer, String, Student> ss = Student::new;
        Student st = ss.apply(60, "surya");
        System.out.println(st.id + "  " + st.name);
    }
}
// Output:
// Constructor called
// 50 jay
// Constructor called
// 60  surya
```