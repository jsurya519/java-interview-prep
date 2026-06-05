## State Design Pattern
It allows an object to change its behavior when its internal state changes. This pattern is particularly useful when an object's behavior depends on its state, and the state can change during the object's lifecycle.

```java
interface VendingMachineState
{
    void handleRequest(VendingMachineContext context);
}

// State 1
class ReadyState implements VendingMachineState
{
    @Override
    public void handleRequest(VendingMachineContext context)
    {
        System.out.println("Machine Ready");

        // Transition to next state
        context.setState(new ProductSelectState());
    }
}

// State 2
class ProductSelectState implements VendingMachineState
{
    @Override
    public void handleRequest(VendingMachineContext context)
    {
        System.out.println("Product Selected");

        // Transition to next state
        context.setState(new PaymentProcessingState());
    }
}

// State 3
class PaymentProcessingState implements VendingMachineState
{
    @Override
    public void handleRequest(VendingMachineContext context)
    {
        System.out.println("Payment Processed");

        // Transition back to Ready State
        context.setState(new ReadyState());
    }
}

// Context
class VendingMachineContext
{
    private VendingMachineState state;

    VendingMachineContext()
    {
        this.state = new ReadyState();
    }

    public void setState(VendingMachineState state)
    {
        this.state = state;
    }

    public void executeRequest()
    {
        state.handleRequest(this);
    }
}

// Client
public class Main
{
    public static void main(String[] args)
    {
        VendingMachineContext machine = new VendingMachineContext();

        machine.executeRequest();
        machine.executeRequest();
        machine.executeRequest();
        machine.executeRequest();
        machine.executeRequest();
    }
}
```