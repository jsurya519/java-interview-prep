## Dependency Inversion Principle
1. High-level modules should not depend on low-level modules. Both should depend on abstractions.
2. Also, abstractions should not depend on details,
but details should depend on abstractions.

DIP suggests that classes should rely on abstractions (e.g., interfaces or abstract classes) rather than concrete implementations.

```java
interface VersionControl
{
    void pull();
    void commit();
    void push();
}

class Git implements VersionControl
{
    @Override
    public void pull()
    {
        System.out.println("pulling code");
    }

    @Override
    public void commit()
    {
        System.out.println("commiting code");
    }

    @Override
    public void push()
    {
        System.out.println("pushing code");
    }
}

class Development
{
    private VersionControl vc;

    Development(VersionControl v)
    {
        this.vc=v;
    }

    void pull()
    {
        vc.pull();
    }

    void commit()
    {
        vc.commit();
    }

    void push()
    {
        vc.push();
    }
}

class DependencyInversion
{
    public static void main(String[] args)
    {
        Development d = new Development(new Git());
        d.pull();
        d.push();
    }
}
```

Here, Development class is High-level module. This contains the business workflow of a developer. Git class is a Low-level module. This is a specific implementation detail. VersionControl is an Abstraction.

**Without DIP:**
Suppose you wrote:
```java
class Development
{
    private Git git = new Git();

    void pull()
    {
        git.pull();
    }
}
```

Now:
```java
Development --> Git
```
It is direct dependency.

Tomorrow if the company moves to another VCS:
```java
class SVN
{
    ...
}
```
You must modify Development.

**With DIP:**
```java
Development --> VersionControl
Git --------> VersionControl
```
Both high-level and low-level modules depend on the abstraction.

---

**Wrong Abstraction**
```java
interface VersionControl
{
    Git getGit();
}
```
Now the abstraction knows about a specific implementation (Git). We should write abstraction like this.