### Throw keyword in Java

The `throw` keyword in Java is used to throw a specific exception.

You can throw either checked or unchecked exceptions in Java using the `throw` keyword. It is primarily used for throwing custom exceptions (user-defined exceptions). We will learn about custom exceptions in the next section.

The syntax for the `throw` keyword is as follows:

```
throw exception_instance;
```

Example: throwing an exception without handling it:

```java
public class ThrowExample {
    static void validate(int age) {
        if (age < 18) {
            throw new ArithmeticException("not valid");
        } else {
            System.out.println("welcome to vote");
        }
    }
    public static void main(String[] args) {
        validate(13);
        System.out.println("rest of the code");
    }
}
```
Output:
```
Exception in thread "main" java.lang.ArithmeticException: not valid
    at ThrowExample.validate(ThrowExample.java:3)
    at ThrowExample.main(ThrowExample.java:9)
```

Example: throwing an exception and handling it:

```java
public class ThrowExample {
    static void validate(int age) {
        if (age < 18) {
            throw new ArithmeticException("not valid");
        } else {
            System.out.println("welcome to vote");
        }
    }
    public static void main(String[] args) {
        try {
            validate(13);
        } catch (ArithmeticException e) {
            System.out.println(e);
        }
        System.out.println("rest of the code");
    }
}
```
Output:
```
java.lang.ArithmeticException: not valid
rest of the code
```

