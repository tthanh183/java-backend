### Throws keyword in Java

The throws keyword in Java is used to declare an exception. It informs the programmer that an exception might occur, so it is better for the programmer to provide exception handling code to maintain the normal flow of the program.

Exception handling is primarily used for handling checked exceptions. If an unchecked exception, such as a NullPointerException, occurs, it is the programmer’s fault for not performing the necessary checks before using the code.

```
return_type method_name() throws exception_class_name {
    // code
}
```

Only checked exceptions should be declared because:

- `Unchecked` exceptions are under your control.
- `Errors` are outside your control, such as VirtualMachineError or StackOverflowError, and you cannot do anything about them.

> [!NOTE]  
> If you call a method that throws an exception, you must either catch the exception or declare it using the `throws` keyword.

Case1: Handling the exception with try/catch  
In this case, the code will execute properly, whether an exception occurs or not in the program.
```java
import java.io.IOException;

class M {
    void method() throws IOException {
        throw new IOException("Device error");
    }
}

public class TestThrows2 {
    public static void main(String args[]) {
        try {
            M m = new M();
            m.method();
        } catch (Exception e) {
            System.out.println("Exception handled");
        }

        System.out.println("Normal flow...");
    }
}
```
Output:
```
Exception handled
Normal flow...
```
Case2: Declaring throws for an exception and  exception does not occur, the code will execute correctly.
```java
import java.io.IOException;

class M {
    void method() throws IOException {
        System.out.println("Device is working fine");
    }
}

public class TestThrows2 {
    public static void main(String args[]) throws IOException {
        M m = new M();
        m.method();
        System.out.println("Normal flow...");
    }
}
```
Output:
```
Device is working fine
Normal flow...
```
Case3: Declaring throws for an exception and the exception occurs, an exception will be thrown at runtime because throws does not handle the exception.
```java
import java.io.IOException;

class M {
    void method() throws IOException {
        throw new IOException("Device error");
    }
}

public class TestThrows2 {
    public static void main(String args[]) throws IOException {
        M m = new M();
        m.method();
        System.out.println("Normal flow...");
    }
}
```
Output:
```
Exception in thread "main" java.io.IOException: Device error
```