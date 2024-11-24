### Continue Keyword in Java
The continue keyword in Java is used to skip the current iteration of a loop and continue with the next iteration. When the continue statement is executed, the remaining code inside the loop after continue is not executed for that iteration, and the loop proceeds to the next cycle. In the case of a nested loop, continue only affects the inner loop.

Example using `continue` with a `for` loop:
```java
public class ContinueExample {
    public static void main(String[] args) {
        for (int i = 1; i <= 10; i++) {
            if (i == 5) {
                continue;  // Skip printing when i == 5
            }
            System.out.println(i);  // Print the value of i
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
6
7
8
9
10
```
Example using `continue` with a nested `for` loop:
```java
public class ContinueExample2 {
    public static void main(String[] args) {
        for (int i = 1; i <= 3; i++) {
            for (int j = 1; j <= 3; j++) {
                if (i == 2 && j == 2) {
                    continue;  // Skip when i == 2 and j == 2
                }
                System.out.println(i + " " + j);  // Print the values of i and j
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
2 3
3 1
3 2
3 3
```