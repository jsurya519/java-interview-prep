## Interface Segregation Principle
This principle is similar to Single Responsibility Principle. It states that "Do not force any client to implement an interface which is irrelevant to them". You should prefer many client interfaces rather than one general interface and each interface should have a specific responsibility.

```java
interface VegMenu
{
    void displayVegMenu();
}

interface NonVegMenu
{
    void displayNonVegMenu();
}

class VegHotel implements VegMenu
{
    @Override
    public void displayVegMenu()
    {
        System.out.println("Displaying veg menu");
    }
}

class NonVegHotel implements NonVegMenu
{
    @Override
    public void displayNonVegMenu()
    {
        System.out.println("Displaying non-veg menu");
    }
}

class MultiCuisineHotel implements VegMenu, NonVegMenu {
    public void displayVegMenu() {
        System.out.println("Displaying veg menu");
    }
    public void displayNonVegMenu() {
        System.out.println("Displaying non-veg menu");
    }
}


class InterfaceSegregation
{
    public static void main(String[] args)
    {
        VegMenu veghotel = new VegHotel();
        veghotel.displayVegMenu();

        NonVegMenu nonVegMenu = new NonVegHotel();
        nonVegMenu.displayNonVegMenu();

    }
}
```