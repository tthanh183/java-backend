### Do-While Loop in Java
1. **Do-While Loop**
The do-while loop in Java is used to repeat a part of the program several times. It is similar to the while loop, except that the do-while loop executes the block of code at least once, even if the condition is false.
```
do {
    // Block of code to be executed
} while (condition);
```
Example:
```java
public class DoWhileExample1 {
    public static void main(String[] args) {
        int a = 1, sum = 0;
        do {
            sum += a;
            a++;
        } while (a <= 5);
        System.out.println("Sum of 1 to 5 is " + sum);
    }
}
```
Output:
```
Sum of 1 to 5 is 15
```
2. **Infinite do-while Loop**

If you set the loop condition to true, the do-while loop will run indefinitely... until you stop the program in your IDE (such as Eclipse or NetBeans) or press Ctrl + C in the command line.
Example:
```java
public class DoWhileExample2 {
    public static void main(String[] args) {
        do {
            System.out.println("Infinite do-while loop...");
        } while (true);
    }
}
```
Output:
```
Infinite do-while loop...
Infinite do-while loop...
Infinite do-while loop...
Infinite do-while loop...
Infinite do-while loop...
...
(Press Ctrl + C to stop)
```