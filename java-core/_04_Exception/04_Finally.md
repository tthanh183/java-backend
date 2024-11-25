### Finally block in Java

The `finally` block in Java is used to execute important code, such as closing connections, closing streams, etc.

The `finally` block in Java is always executed regardless of whether an exception occurs or not, or even if a `return` statement is encountered in the try block.

The `finally` block in Java is declared after the `try` block or after the `catch` block.

> [!NOTE]  
>  If you do not handle an exception, the JVM will still execute the finally block (if present) before the program ends.

Example1: using `finally` block without no exception occurs:
```java
public class FinallyExample {
    public static void main(String[] args) {
        try {
            int data = 25 / 5;
            System.out.println(data);
        } catch (NullPointerException e) {
            System.out.println(e);
        } finally {
            System.out.println("finally block is always executed");
        }
        System.out.println("rest of the code");
    }
}
```
Output:
```
5
finally block is always executed
rest of the code
```

Example2: using `finally` block with an exception occurs but not handled:
```java
public class FinallyExample {
    public static void main(String[] args) {
        try {
            int data = 25 / 0; // may throw an exception
            System.out.println(data);
        } catch (NullPointerException e) {
            System.out.println(e);
        } finally {
            System.out.println("finally block is always executed");
        }
        System.out.println("rest of the code");
    }
}
```
Output:
```
finally block is always executed
Exception in thread "main" java.lang.ArithmeticException: / by zero
    at FinallyExample.main(FinallyExample.java:4)
```

Example3: using `finally` block with an exception occurs and handled:
```java

public class FinallyExample {
    public static void main(String[] args) {
        try {
            int data = 25 / 0; // may throw an exception
            System.out.println(data);
        } catch (ArithmeticException e) {
            System.out.println(e);
        } finally {
            System.out.println("finally block is always executed");
        }
        System.out.println("rest of the code");
    }
}
```
Output:
```
finally block is always executed
java.lang.ArithmeticException: / by zero
rest of the code
```

Example4: using `finally` block when a return statement is encountered:
```java
public class FinallyExample {
    public static void main(String[] args) {
        System.out.println(getData());
    }
    public static int getData() {
        try {
            return 10;
        } finally {
            System.out.println("finally block is always executed");
        }
    }
}
```
Output:
```
finally block is always executed
10
```

![!NOTE]
> The finally block will not execute if the program terminates (e.g., by calling System.exit()) or if an error occurs that causes the program to crash. 



