### For loop
The for loop in Java is used to repeat a part of the program multiple times. If the number of iterations is fixed, the for loop is recommended. However, if the number of iterations is not fixed, it is better to use a while or do-while loop.

There are three types of for loops in Java:
- Simple for loop
- Enhanced for loop
- Labeled for loop

1. **Simple for Loop**

The simple for loop in Java is similar to the one in C/C++. You can initialize a variable, check a condition, and increment or decrement the value of the variable.
```
for (initialization; condition; increment/decrement) {
    // Block of code to be executed
}

```
Example:
```java
public class ForExample {
    public static void main(String[] args) {
        for (int i = 1; i <= 10; i++) {
            System.out.print(i);
        }
    }
}
```
Output:
```
1 2 3 4 5 6 7 8 9 10
```
2. **Enhanced for Loop**

The enhanced for loop is used to iterate over arrays or collections in Java. It is easier to use than the simple for loop because you don't need to manually increment/decrement the variable or check the condition. You simply use the colon `:` symbol.
```
for (Type var : array) {
    // Block of code to be executed
}
```
Example:
```java
public class ForEachExample {
    public static void main(String[] args) {
        int[] arr = {12, 23, 44, 56, 78};
        for (int i : arr) {
            System.out.print(i);
        }
    }
}
```
Output:
```
12, 23, 44, 56, 78
```
3. **Labeled for Loop**

You can assign a label to each for loop by placing a label before the for loop. This is useful when you want to break or continue (exit/skip) specific loops.
```
label_name:
for (initialization; condition; increment/decrement) {
    // Block of code to be executed
}
```
Example:
```java
public class LabeledForExample {
    public static void main(String[] args) {
        aa: for (int i = 1; i <= 3; i++) {
            bb: for (int j = 1; j <= 3; j++) {
                if (i == 2 && j == 2) {
                    break aa;
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
```