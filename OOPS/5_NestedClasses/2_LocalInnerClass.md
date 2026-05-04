## Local Inner Class:
A Local Inner Class in Java is a class defined inside a method, a constructor, or a block and is used only within that limited scope.
1. Declared inside a method, constructor, or block; scope is limited to that block
2. Can access outer class members and final / effectively final local variables
3. Cannot use access modifiers (public, private, protected)
4. Cannot have static members (except static final constants)

```java
class Outer {
    private int x = 10;

    void outerMethod() {
        int y = 20; // effectively final

        class LocalInner {
            void print() {
                System.out.println("x = " + x);
                System.out.println("y = " + y);
            }
        }

        LocalInner obj = new LocalInner();
        obj.print();
    }
}

public class Main {
    public static void main(String[] args) {
        Outer o = new Outer();
        o.outerMethod();
    }
}
// Output:
// x = 10
// y = 20
```

