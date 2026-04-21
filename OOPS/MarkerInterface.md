A marker interface is a interface that contains no methods and fields. It is used to mark a class so that jvm can identify it has some special behaviour.

**Common Marker Interfaces:**
1. Cloneable Interface
2. Serializable Interface

```java

interface Cloneable {
	// Marker Interface
}

interface Serializable {
  	// Marker Interface
}
```

**Cloneable Interface:**
It helps to clone an object using Object.clone(). If class doesn't implement Cloneable interface, then clone() throws CloneNotSupportedException. clone() provides only shallow copy.

```java
class Employee implements Cloneable
{
    String name;
    int age;

    Employee(String name, int age)
    {
        this.name=name;
        this.age=age;
    }

    @Override
    public Employee clone() throws CloneNotSupportedException {
    return (Employee) super.clone();
    }

}

public class Main
{
    public static void main(String[] args) throws CloneNotSupportedException
    {
        Employee e1 = new Employee("abc", 50);
        Employee e2 = e1.clone();
    }
}
```



---

**Serializable Interface:**

Serializable Interface helps to serialize the objects of the class. Serialize means to convert the objects into byteStream, so that we can store the state of the object in the file or DB. Later we can deserialize to the convert the byteStream to actual object.

If we didn't implement Serializable interface, then it throws NotSerializableException.

```java
import java.io.*;

class Employee implements Serializable
{
    String name;
    int age;

    Employee(String name, int age)
    {
        this.name=name;
        this.age=age;
    }
}

public class CheckSerializable {

    public static void main(String[] args) throws IOException, ClassNotFoundException {
        Employee e1 = new Employee("abc", 50);

        //Serialization
        FileOutputStream fs = new FileOutputStream("xyz.txt");
        ObjectOutputStream os = new ObjectOutputStream(fs);

        os.writeObject(e1);

        //Deserializable
        FileInputStream fis = new FileInputStream("xyz.txt");
        ObjectInputStream ois = new ObjectInputStream(fis);

        Employee e2 = (Employee) ois.readObject();

        System.out.println(e2.age + " "+ e2.name);

        os.close();
        ois.close();

    }
}

```
