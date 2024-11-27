### Try-Catch with Resources

The try-with-resources statement in Java 7 is a statement that declares one or more resources. A resource is an object that must be closed after the program has finished using it. The try-with-resources statement ensures that each resource is closed after the statement executes.

You can pass any object that implements java.lang.AutoCloseable, which includes all objects that implement java.io.Closeable.

Example:

```java
import java.io.*;

public class TryWithResourcesExample {
    public static void main(String args[]) {
        try (FileReader fr = new FileReader("test.txt")) {
            char[] a = new char[50];
            fr.read(a); // reads the content to the array
            for (char c : a)
                System.out.print(c); // prints the characters one by one
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```
Output:

```
This is a test file.
```

