## Single Responsibility Principle

**"A class should have only one reason to change"** which means every class should have a single responsibility or single job or single purpose.

```java
class BreadMaker
{
    void makeBread()
    {
        System.out.println("Making bread");
    }
}

class SupplyOrder
{
    void supplyOrder()
    {
        System.out.println("Supplying order");
    }
}

class CustomerService
{
    void serveCustomer()
    {
        System.out.println("serving customer");
    }
}

class BakeryManager {
    private BreadMaker breadMaker = new BreadMaker();
    private SupplyOrder supplyOrder = new SupplyOrder();
    private CustomerService customerService = new CustomerService();

    void runBakery() {
        breadMaker.makeBread();
        supplyOrder.supplyOrder();
        customerService.serveCustomer();
    }
}

public class SingleResponsibility {
    public static void main(String[] args) {
        new BakeryManager().runBakery();
    }
}
```

**A violation of SRP would look like this**
```java
class BakeryManager {

    void makeBread() {
        System.out.println("Making bread");
    }

    void supplyOrder() {
        System.out.println("Supplying order");
    }

    void serveCustomer() {
        System.out.println("Serving customer");
    }

    void calculateSalary() {
        System.out.println("Calculating salary");
    }

    void generateTaxReport() {
        System.out.println("Generating tax report");
    }
}
```