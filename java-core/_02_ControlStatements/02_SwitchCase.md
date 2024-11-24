### Switch-Case Statement in Java
The switch-case statement in Java is used to execute one or more blocks of code based on multiple conditions.
```
switch (expression) {    
    case value_1:
        // Block of code 1
        break;  // optional
    case value_2:    
        // Block of code 2
        break;  // optional
    ...
    case value_n:    
        // Block of code n
        break;  // optional    
    default:     
        // This block of code is executed
        // if none of the above conditions are satisfied
}
```
Example:
```java
public class SwitchExample {
    public static void main(String[] args) {
        int number = 20;
        switch (number) {
            case 10:
                System.out.println("10");
                break;
            case 20:
                System.out.println("20");
                break;
            case 30:
                System.out.println("30");
                break;
            default:
                System.out.println("Not in 10, 20 or 30");
        }
    }
}
```
Output:
```
20
```
**Switch-Case Without Using 'Break'**

If the break keyword is not used in a switch-case statement, the program will continue executing the blocks of code for subsequent case labels, even if they do not match the original condition (known as "fall-through").

Example:
```java
public class SwitchExample2 {
    public static void main(String[] args) {
        int number = 20;
        switch (number) {
            case 10:
                System.out.println("10");
            case 20:
                System.out.println("20");
            case 30:
                System.out.println("30");
            default:
                System.out.println("Not in 10, 20 or 30");
        }
    }
}
```
Output:
```
20
30
Not in 10, 20 or 30
```