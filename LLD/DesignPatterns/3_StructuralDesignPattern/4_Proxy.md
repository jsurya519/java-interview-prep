## Proxy Design Pattern
Proxy Design Pattern is a structural design pattern where a proxy object acts as a placeholder to control access to the real object. The client communicates with the proxy, which forwards requests to the real object. The proxy can also provide extra functionality such as access control, lazy initialization, logging, and caching.

```java
// Subject interface
interface Image {
    void display();
}

// Real object
class RealImage implements Image {
    private String filename;

    RealImage(String filename) {
        this.filename = filename;
        loadFromDisk();
    }

    private void loadFromDisk() {
        System.out.println("Loading " + filename + " from disk...");
    }

    @Override
    public void display() {
        System.out.println("Displaying " + filename);
    }
}

// Proxy
class ProxyImage implements Image {
    private RealImage realImage;
    private String filename;

    ProxyImage(String filename) {
        this.filename = filename;
    }

    @Override
    public void display() {
        if (realImage == null) {          // lazy initialization
            realImage = new RealImage(filename);
        }
        realImage.display();
    }
}

// Client
public class ProxyPattern {
    public static void main(String[] args) {
        Image img1 = new ProxyImage("photo1.jpg");
        Image img2 = new ProxyImage("photo2.jpg");

        img1.display(); // loads and displays photo1.jpg
        img1.display(); // displays photo1.jpg without loading again

        img2.display(); // loads and displays photo2.jpg
    }
}

```