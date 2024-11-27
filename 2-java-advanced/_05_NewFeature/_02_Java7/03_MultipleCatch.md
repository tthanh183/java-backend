### Multiple Catch

Java allows you to catch multiple types of exceptions in a single catch block. This was introduced in Java 7 and helps optimize code.

You can use the vertical bar (|) to separate multiple exceptions in the catch block.

Here is an older way, before Java 7, to handle multiple exceptions.
    
```java
public class MultipleExceptionExample1 {
    public static void main(String args[]) {
        try {
            int array[] = new int[10];
            array[10] = 30 / 0;
        } catch (ArithmeticException e) {
            System.out.println(e.getMessage());
        } catch (ArrayIndexOutOfBoundsException e) {
            System.out.println(e.getMessage());
        } catch (Exception e) {
            System.out.println(e.getMessage());
        }
    }
}
```
Output:
```
/ by zero
```

Here is the new way, after Java 7, to handle multiple exceptions.

```java

public class MultipleExceptionExample2 {
    public static void main(String args[]) {
        try {
            int array[] = new int[10];
            array[10] = 30 / 0;
        } catch (ArithmeticException | ArrayIndexOutOfBoundsException e) {
            System.out.println(e.getMessage());
        } catch (Exception e) {
            System.out.println(e.getMessage());
        }
    }
}
```
Output:
```
/ by zero
```

> [!NOTE]  
> The catch block with multiple exceptions must be ordered from the most specific to the most general exception. If you reverse the order, you will get a compilation error.

Example:

```java
public class MultipleExceptionExample3 {
    public static void main(String args[]) {
        try {
            int array[] = new int[10];
            array[10] = 30 / 0;
        } catch (Exception | ArithmeticException | ArrayIndexOutOfBoundsException e) {
            System.out.println(e.getMessage());
        }
    }
}
```
Output:
```
Compile time error: Alternatives in a multi-catch statement cannot be related by subclassing
```

> [!NOTE]
> The argument of the catch block is implicitly final. You cannot change the value of the argument inside the catch block.
