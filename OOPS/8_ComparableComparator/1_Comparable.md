## Comparable Interface:
It defines the natural ordering of objects of a class. It enables objects to be compared and sorted automatically without using an external Comparator.

1. It contains the compareTo() method, which compares the current object with another object.
2. It is commonly used with Collections.sort() and Arrays.sort() for sorting custom objects.


```java
public interface Comparable<T> {
    int compareTo(T o);
}
```

```java
import java.util.*;

class Student implements Comparable<Student>
{
    private int id;
    private String name;

    Student(int id, String name)
    {
        this.id=id;
        this.name=name;
    }

    @Override
    public int compareTo(Student s)
    {
        if(this.id <= s.id)
            return -1;
		else if(this.id == s.id)
			return 0;
        return 1;
    }

	// The above method can also written in a simple way
	// @Override
 //    public int compareTo(Student s)
 //    {
 //        return this.id - s.id;
 //    }

    static void print(List<Student> list)
    {
        for(Student s:list)
        {
            System.out.println(s.id + " "+s.name);
        }
    }
}

public class Dummy {
    public static void main(String[] args) {
        List<Student> list = new ArrayList<>();
        list.add(new Student(10, "jay"));
        list.add(new Student(5, "san"));
        list.add(new Student(1, "vin"));
        list.add(new Student(100, "mad"));

        Collections.sort(list);
        Student.print(list);

        Comparable kk;
        Integer ll;
    }
}
// output:
// 1 vin
// 5 san
// 10 jay
// 100 mad
```


## Understanding compareTo():
compareTo() returns a negative, zero, or positive value to indicate order.

compareTo() is used to compare two objects.
```java
obj1.compareTo(obj2);
```
It returns an integer value.

**For ascending order:**

| Return Value | Meaning                   |
| ------------ | ------------------------- |
| Negative (-) | obj1 is smaller than obj2 |
| Zero (0)     | obj1 is equal to obj2     |
| Positive (+) | obj1 is greater than obj2 |

**For descending order:**

| Return Value | Meaning                   |
| ------------ | ------------------------- |
| Negative (-) | obj1 is greater than obj2 |
| Zero (0)     | obj1 is equal to obj2     |
| Positive (+) | obj1 is lesser than obj2 |


**Example:**

[10,20,20,30]

```java
Ascending Order:
----------------
10.compareTo(20) → negative
20.compareTo(20) → zero
30.compareTo(20) → positive

Descending Order (reversed comparison):
---------------------------------------
compare(10,20) → positive
compare(20,20) → zero
compare(30,20) → negative
```

---

```java
public class Dummy {
    public static void main(String[] args) {
        int[] a = new int[4];
        a[0]= 100;
        a[1]= 10;
        a[2]=50;
        a[3]=66;

        Arrays.sort(a);

        List<Integer> l = new ArrayList<>();
        l.add(100);
        l.add(10);
        l.add(50);
        l.add(66);

        Collections.sort(l);

    }
}
```