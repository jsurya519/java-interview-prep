Provide a simplified interface to a complex subsystem.
The client interacts with the facade instead of dealing with multiple subsystems directly.

```java
interface Hotel {
    void printMenu();
}

class VegHotel implements Hotel {
    @Override
    public void printMenu() {
        System.out.println("Veg menu");
    }
}

class NonVegHotel implements Hotel {
    @Override
    public void printMenu() {
        System.out.println("Non-Veg menu");
    }
}

class BothHotel implements Hotel {
    @Override
    public void printMenu() {
        System.out.println("Veg + Non-Veg menu");
    }
}

// Facade
class HotelFacade {
    private Hotel veg;
    private Hotel nonVeg;
    private Hotel both;

    HotelFacade() {
        veg = new VegHotel();
        nonVeg = new NonVegHotel();
        both = new BothHotel();
    }

    void printVegMenu() {
        veg.printMenu();
    }

    void printNonVegMenu() {
        nonVeg.printMenu();
    }

    void printBothMenu() {
        both.printMenu();
    }
}

// Client
public class FacadePattern {
    public static void main(String[] args) {
        HotelFacade hotel = new HotelFacade();
        hotel.printVegMenu();
        hotel.printNonVegMenu();
        hotel.printBothMenu();
    }
}

```