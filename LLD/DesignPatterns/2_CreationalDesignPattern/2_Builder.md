Builder design pattern provides a step-by-step approach to construct complex objects. It separates the construction process and enabling to create different variations of an object.

```java
class Computer
{
    String cpu;
    String ram;
    String memory;

    private Computer(Builder b)
    {
        this.cpu=b.cpu;
        this.ram=b.ram;
        this.memory=b.memory;
    }

    static class Builder
    {
        String cpu;
        String ram;
        String memory;

        Builder()
        {

        }

        Builder setCpu(String cpu)
        {
            this.cpu=cpu;
            return this;
        }

        Builder setRam(String ram)
        {
            this.ram=ram;
            return this;
        }

        Builder setMemory(String memory)
        {
            this.memory=memory;
            return this;
        }

        Computer build()
        {
            return new Computer(this);
        }
    }
}

class BuilderPattern
{
    public static void main(String[] args)
    {
        Computer c = new Computer.Builder().setCpu("cpu").setMemory("5GB").setRam("1GB").build();

    }
}
```