## Command Design Pattern
Command Design Pattern is a behavioral pattern that encapsulates a request as an object, decoupling the sender from the receiver. It allows requests to be queued, logged, parameterized, or undone/redone, providing flexibility and extensibility in executing operations.

```java
// 1️⃣ Command Interface
public interface Command {
    void execute();
}

// 2️⃣ Concrete Commands
public class LightOnCommand implements Command {
    private Light light;

    public LightOnCommand(Light light) {
        this.light = light;
    }

    @Override
    public void execute() {
        light.turnOn();
    }
}

public class LightOffCommand implements Command {
    private Light light;

    public LightOffCommand(Light light) {
        this.light = light;
    }

    @Override
    public void execute() {
        light.turnOff();
    }
}

// 3️⃣ Receiver
public class Light {
    public void turnOn() {
        System.out.println("Light is ON");
    }

    public void turnOff() {
        System.out.println("Light is OFF");
    }
}

// 4️⃣ Invoker
public class RemoteControl {
    private Command command;

    public void setCommand(Command command) {
        this.command = command;
    }

    public void pressButton() {
        command.execute();
    }
}

// 5️⃣ Client
public class Main {
    public static void main(String[] args) {
        Light livingRoomLight = new Light();

        // Create commands
        Command lightOn = new LightOnCommand(livingRoomLight);
        Command lightOff = new LightOffCommand(livingRoomLight);

        // Invoker
        RemoteControl remote = new RemoteControl();

        // Turn ON the light
        remote.setCommand(lightOn);
        remote.pressButton(); // Output: Light is ON

        // Turn OFF the light
        remote.setCommand(lightOff);
        remote.pressButton(); // Output: Light is OFF
    }
}
```