## Decorator Design Pattern
Decorator Design Pattern enables the dynamic addition of functionality to specific objects without changing the behavior of other objects in the same class.

```java
// Base interface
interface Coffee {
    int getPrice();
    String getDesc();
}

// Concrete component
class PlainCoffee implements Coffee {
    private final int price;
    private final String desc;

    PlainCoffee(int price, String desc) {
        this.price = price;
        this.desc = desc;
    }

    @Override
    public int getPrice() {
        return this.price;
    }

    @Override
    public String getDesc() {
        return this.desc;
    }
}

// Base decorator
class Decorator implements Coffee {
    protected Coffee coffee;

    Decorator(Coffee coffee) {
        this.coffee = coffee;
    }

    @Override
    public int getPrice() {
        return coffee.getPrice();
    }

    @Override
    public String getDesc() {
        return coffee.getDesc();
    }
}

// Concrete decorators
class MilkDecorator extends Decorator {
    MilkDecorator(Coffee coffee) {
        super(coffee);
    }

    @Override
    public int getPrice() {
        return coffee.getPrice() + 5;
    }

    @Override
    public String getDesc() {
        return coffee.getDesc() + " + Milk";
    }
}

class SugarDecorator extends Decorator {
    SugarDecorator(Coffee coffee) {
        super(coffee);
    }

    @Override
    public int getPrice() {
        return coffee.getPrice() + 5;
    }

    @Override
    public String getDesc() {
        return coffee.getDesc() + " + Sugar";
    }
}

// Client
public class DecoratorPattern {
    public static void main(String[] args) {
        // Plain coffee
        Coffee plain = new PlainCoffee(5, "Plain coffee");
        System.out.println(plain.getDesc() + " : " + plain.getPrice());

        // Coffee with milk
        Coffee milkCoffee = new MilkDecorator(plain);
        System.out.println(milkCoffee.getDesc() + " : " + milkCoffee.getPrice());

        // Coffee with milk and sugar (chaining)
        Coffee fancyCoffee = new SugarDecorator(milkCoffee);
        System.out.println(fancyCoffee.getDesc() + " : " + fancyCoffee.getPrice());
    }
}

```