## Open/Close Principle
**"Open for extension, but closed for modification"** which means you should be able to extend a class behavior without modifying it.

```java
interface Payments
{
    void processPayment(Double amount);
}

class DebitCard implements Payments
{
    @Override
    public void processPayment(Double amount)
    {
        System.out.println("Processing via Debit card - "+amount);
    }
}

class CreditCard implements Payments
{
    @Override
    public void processPayment(Double amount)
    {
        System.out.println("Processing via credit card - "+amount);
    }
}

class PaymentProcessor
{
    private final Payments payment;

    PaymentProcessor(Payments p)
    {
        this.payment=p;
    }

    void makePayment(Double amount)
    {
        payment.processPayment(amount);
    }
}

class OpenClosed
{
    public static void main(String[] args)
    {
        PaymentProcessor pp = new PaymentProcessor(new CreditCard());
        pp.makePayment(70d);
    }
}

```

In above design, tomorrow if any other new Payment like UPI can also be accommodated without modifying the PaymentProcesser.

**Violation for Open/Close Principle:**
```java
class PaymentProcessor
{
    void makePayment(String type, Double amount)
    {
        if(type.equals("CREDIT"))
        {
            System.out.println("Processing Credit Card payment: " + amount);
        }
        else if(type.equals("DEBIT"))
        {
            System.out.println("Processing Debit Card payment: " + amount);
        }
        else
        {
            throw new IllegalArgumentException("Unsupported payment type");
        }
    }
}

public class Main
{
    public static void main(String[] args)
    {
        PaymentProcessor pp = new PaymentProcessor();

        pp.makePayment("CREDIT", 100.0);
        pp.makePayment("DEBIT", 50.0);
    }
}
```