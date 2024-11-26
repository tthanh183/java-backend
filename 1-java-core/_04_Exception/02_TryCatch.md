### Try-Catch Block in Java

The `try` block in Java is used to contain code that might throw an exception. It must be declared within a method.

After a `try` block, you must declare either a `catch` block, a `finally` block, or both.

```
try {  
    // code that may throw an exception
} catch(Exception_class_Name ref) {
    // exception handling code
} finally {
    // code that will always execute
}
```
> [!NOTE]  
> -You can have multiple `catch` blocks for a single `try` block.  
> -The `finally` block is optional. It will always execute, whether an exception is thrown or not.

Example of not using a `try-catch` block:
```java
public class TryCatchExample {
    public static void main(String[] args) {
        int data = 50 / 0; // may throw an exception
    }
}
```
Output:
```
Exception in thread "main" java.lang.ArithmeticException: / by zero
    at TryCatchExample.main(TryCatchExample.java:4)
```
Example of using a `try-catch` block:
```java
public class TryCatchExample {
    public static void main(String[] args) {
        try {
            int data = 50 / 0; // may throw an exception
        } catch (ArithmeticException e) {
            System.out.println(e);
        }
        System.out.println("rest of the code");
    }
}
```
Output:
```
java.lang.ArithmeticException: / by zero
rest of the code
```

### Nested Try-Catch Block in Java

In Java, you can use a `try-catch` block within another `try-catch` block. This is known as a nested `try-catch` block.

Example:
```java
public class NestedTryCatchExample {
    public static void main(String[] args) {
        try {
            try {
                int arr[] = {1, 2, 3, 4};
                System.out.println(arr[10]); // may throw an exception
            } catch (ArithmeticException e) {
                System.out.println("Arithmetic Exception occurs");
            }
        } catch (ArrayIndexOutOfBoundsException e) {
            System.out.println("ArrayIndexOutOfBounds Exception occurs");
        } catch (Exception e) {
            System.out.println("Parent Exception occurs");
        }
        System.out.println("rest of the code");
    }
}
```
