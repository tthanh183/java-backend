### Break Keyword in Java
1. **Using Break in Java**

The break keyword in Java is used to stop the execution of a loop or a switch statement when a specified condition is met. For nested loops, it only terminates the inner loop where the break is called.
Example:
```java
public class BreakExample {
    public static void main(String[] args) {
        for (int i = 1; i <= 10; i++) {
            if (i == 5) {
                break;
            }
            System.out.println(i);
        }
    }
}
```
Output:
```
1
2
3
4
```
Example of using `Break` in a Nested for Loop
```java
public class BreakExample2 {
    public static void main(String[] args) {
        for (int i = 1; i <= 3; i++) {
            for (int j = 1; j <= 3; j++) {
                if (i == 2 && j == 2) {
                    break;
                }
                System.out.println(i + " " + j);
            }
        }
    }
}
```
Output:
```
1 1
1 2
1 3
2 1
3 1
3 2
3 3
```
2. **Using Break with a switch-case Statement**

To understand how to use `break` with a switch-case statement, you can refer to this lesson on the [switch-case statement in Java](02_SwitchCase.md#switch-case-statement).
