### If-Else statement in Java
The "if" Statement in Java is used to evaluate a boolean condition. It returns either True or False. There are different forms of the if-else statement in Java, as follows:
- If statement
- If-Else Statement
- If-Else-If Statement

1. **If Statement**

The if statement is used to evaluate the boolean value of a condition. The block of code following the if will execute if the condition evaluates to True.
```
if(condition) {
    // this block of code executes
    // if condition is true
}
```
Example:
```Java
public class Test {
    public static void main(String[] args) {
        int age = 20;
        if (age > 18) {
            System.out.print("Age is greater than 18");
        }
    }
}
```
Output:
```
Age is greater than 18
```
2. **If-Else Statement**  

The if-else statement also evaluates a boolean condition. If the condition is True, the block following the if will execute. If the condition is False, only the block following the else will execute.
```
if (condition) {
    // this block of code executes
    // if condition is true
} else {
    // this block of code executes
    // if condition is false
}
```
Example:
```java
public class Test {
    public static void main(String[] args) {
        int number = 13;
        if (number % 2 == 0) {
            System.out.println("The number " + number + " is even.");
        } else {
            System.out.println("The number " + number + " is odd.");
        }
    }
}
```
Output:
```
The number 13 is odd.
```
3. **If-Else-If Statement**

The if-else-if statement evaluates multiple boolean conditions. If the first if condition is True, the block following that if will execute. If the first condition is False, the program evaluates subsequent else if conditions. If none of the conditions evaluate to True, the block of code following the final else will execute.
```
if (condition1) {
    // this block of code executes
    // if condition1 is true  
} else if (condition2) {
    // this block of code executes
    // if condition2 is true  
} else if (condition3) {
    // this block of code executes
    // if condition3 is true  
} 
...  
else {  
    // this block of code executes
    // if all conditions above are false
}
```
Example:
```java
public class Test {
    public static void main(String[] args) {
        int marks = 65;

        if (marks < 50) {
            System.out.println("Fail!");
        } else if (marks >= 50 && marks < 60) {
            System.out.println("Grade D");
        } else if (marks >= 60 && marks < 70) {
            System.out.println("Grade C");
        } else if (marks >= 70 && marks < 80) {
            System.out.println("Grade B");
        } else if (marks >= 80 && marks < 90) {
            System.out.println("Grade A");
        } else if (marks >= 90 && marks < 100) {
            System.out.println("Grade A+");
        } else {
            System.out.println("Invalid value!");
        }
    }
}
```
Output:
```
Grade C
```