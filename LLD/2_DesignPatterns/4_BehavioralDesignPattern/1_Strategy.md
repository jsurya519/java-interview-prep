## Strategy Design Pattern
It allows to define a family of algorithms or behaviors, put each of them in a separate class, and make them interchangeable at runtime. This pattern is useful when you want to dynamically change the behavior of the class without modifying its code.

```java
public interface PaymentStrategy
{
    void makePayment();
}

public class CardPayment implements PaymentStrategy
{
    @Override
    public void makePayment()
    {
        System.out.println("Payment made via Card");
        //business logic
    }
}

public class UpiPayment implements PaymentStrategy
{
    @Override
    public void makePayment()
    {
        System.out.println("Payment made via UPI");
        //business logic
    }
}


public class PaymentContext
{
    private PaymentStrategy ps;

    PaymentContext(PaymentStrategy ps)
    {
        this.ps=ps;
    }

    public void setPaymentStrategy(PaymentStrategy ps)
    {
        this.ps=ps;
    }

    public void pay()
    {
        ps.makePayment();
    }
}

public class Main {
    public static void main(String[] args) {
        // Pay using Card
        PaymentStrategy cardPayment = new CardPayment();
        PaymentContext paymentContext = new PaymentContext(cardPayment);
        paymentContext.pay();

        // Switch strategy to UPI dynamically
        PaymentStrategy upiPayment = new UpiPayment();
        paymentContext.setPaymentStrategy(upiPayment);
        paymentContext.pay();
    }
}

```