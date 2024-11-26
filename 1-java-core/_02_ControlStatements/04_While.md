### While Loop in Java
1. **While Loop**
The while loop in Java is used to repeat a part of the program several times. If the number of iterations is not predetermined, the while loop is recommended to use in this case.
```
while (condition) {
    // Block of code that repeats until condition is False
}
```
Example:
```java
public class WhileExample1 {
    public static void main(String[] args) {
        int i = 1;
        while (i <= 10) {
            System.out.print(i);
            i++;
        }
    }
}

```
Output:
```
1 2 3 4 5 6 7 8 9 10
```
2. **Infinite While Loop**

If you set the loop condition to true, the while loop will run indefinitely... until you stop the program in your IDE (Eclipse, NetBeans, etc.) or press Ctrl + C when running from the command line.

Example:
```java
public class WhileExample2 {
    public static void main(String[] args) {
        while (true) {
            System.out.println("Infinite while loop...");
        }
    }
}
```
Output:
```
Infinite while loop...
Infinite while loop...
Infinite while loop...
Infinite while loop...
Infinite while loop...
...
(Press Ctrl + C to stop)
```

