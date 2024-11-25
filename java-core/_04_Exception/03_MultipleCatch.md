### Multiple Catch Blocks in Java

If you need to perform different tasks where different exceptions may occur, you should use multiple catch blocks in Java.

Example:
```java
public class MultipleCatchExample {
    public static void main(String[] args) {
        try {
            int[] arr = new int[5];
            arr[5] = 30 / 0;
        } catch (ArithmeticException e) {
            System.out.println("Arithmetic Exception occurs");
        } catch (ArrayIndexOutOfBoundsException e) {
            System.out.println("ArrayIndexOutOfBounds Exception occurs");
        } catch (Exception e) {
            System.out.println("Parent Exception occurs");
        }
        System.out.println("rest of the code");
    }
}
```
Output:
```
ArrayIndexOutOfBounds Exception occurs
rest of the code
```

> [!NOTE]
> - At any given time, only one exception is occurred and only one catch block is executed.
> - All catch blocks must be ordered from most specific to most general, i.e., catch for `ArithmeticException` must come before `Exception`. If not, you will get a compilation error.
