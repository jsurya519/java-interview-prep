**Serialization:**
Serialization is a mechanism to convert the object into the byteStream. By converting into a byteStream we can store the state of the Object into DB, file, etc..

The class must implement Serializable Interface to serialize the objects of the class.

**Deserialization:**
Deserialization is a reverse process to convert the byteStream into Object of the class.

<img width="629" height="417" alt="image" src="https://github.com/user-attachments/assets/4964ecd7-2bdd-4b3f-84ca-fbe500a5a72f" />


**Points to remember while using Serialization:**
1. **Parent-Child Serialization:** If a parent class has implemented Serializable interface then child class doesn't need to implement it. But vice-versa is not true.
2. **Non-static Data Members:** Only non-static data members are saved via Serialization process. Static variable are not serialized becaue they are not associated with any specific instance.
3. **Transient Data Members:** Static data members and transient data members are not saved via Serialization process. So, if you don't want to save value of a non-static data member then make it transient.
4. **Constructor Calling:** Constructor of object is never called when an object is deserialized.
5. **Associated Objects:** Associated objects must be implementing Serializable interface. If associated Objects is not serializable, then it throws NotSerializableException.

Associated Objects basically means "has-a relationship". Any object that is referenced as a field inside another object.

Ex: Employee has Address.

```java
import java.io.*;
import java.net.spi.InetAddressResolver;


class Address implements Serializable
{
    int doorNo;
    String street;

    Address(int doorNo, String street)
    {
        this.doorNo=doorNo;
        this.street=street;
    }
}

class Employee implements Serializable
{
    static String desination;
    String name;
    int age;
    transient int pass;
    Address add;

    Employee(String name, int age, int pass, Address a1)
    {
        this.name=name;
        this.age=age;
        this.pass=pass;
        this.add=a1;
    }
}

public class CheckSerializable {

    public static void main(String[] args) throws IOException, ClassNotFoundException {

        Address a1 = new Address(90, "ashok-nagar");
        Employee e1 = new Employee("abc", 50, 100, a1);
        Employee.desination = "Engineer";

        //Serialization
        FileOutputStream fs = new FileOutputStream("xyz.txt");
        ObjectOutputStream os = new ObjectOutputStream(fs);

        os.writeObject(e1);

        //Deserializable
        FileInputStream fis = new FileInputStream("xyz.txt");
        ObjectInputStream ois = new ObjectInputStream(fis);

        Employee e2 = (Employee) ois.readObject();

        System.out.println(e2.age + " "+ e2.name+" "+e2.pass + " "+e2.add.doorNo + " "+e2.add.street);

        os.close();
        ois.close();

    }
}

```


---
**SerialVersionUID:**
SerialVersionUID is used as a version identifier for the class during serialization. If the version of class didn't match at the time of deserialization, then it throws InvalidClassException.

By default, JVM generates serialVersionUID based on class structure.
To define our own, we use:
```java
private static final long serialVersionUID = 3L;
```
