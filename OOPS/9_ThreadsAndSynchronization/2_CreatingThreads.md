## Creating Threads:
We can create threads in java using two ways
![66d35e496639898e5f7130b135e8cd4b.png](:/2778e959434f4a28b54946bc74a43d1c)

1. Extending Thread Class
2. Implementing a Runnable interface

**1. By Extending Thread Class**

```java
class Employee extends Thread
{
    public void run(){
        System.out.println("Current thread is " +
                Thread.currentThread().getName());
    }
}

public class Dummy {

    public static void main(String[] args){

        System.out.println(Thread.currentThread().getName());

        Thread t1 = new Employee();
        Thread t2 = new Employee();
        Thread t3 = new Employee();

        t1.start();
        t2.start();
        t3.start();

        System.out.println(Thread.currentThread().getName());
    }
}
// Output:
// main
// main
// Current thread is Thread-2
// Current thread is Thread-0
// Current thread is Thread-1
```


**2. Using Runnable Interface**

```java
import java.io.*;
import java.util.*;

class MyThread implements Runnable{

  	// Method to start Thread
    public void run(){

      	String str = "Thread is Running Successfully";
        System.out.println(str);
    }

}

public class Geeks{

    public static void main(String[] args){

        MyThread g1 = new MyThread();

        // initializing Thread Object
        Thread t1 = new Thread(g1);

      	// Running Thread
      	t1.start();
    }
}
```


```java
public class Dummy {

    public static void main(String[] args){

        Thread t1 = new Thread(new Runnable() {
            @Override
            public void run() {
                System.out.println("Current thread is " +
                        Thread.currentThread().getName());
            }
        });

        Thread t2 = new Thread(()-> System.out.println("Current thread is " +
                Thread.currentThread().getName()));

        t1.start();
        t2.start();
    }
}
// Output:
// Current thread is Thread-0
// Current thread is Thread-1
```

