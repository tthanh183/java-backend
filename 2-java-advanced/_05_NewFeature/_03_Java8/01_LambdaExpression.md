### Lambda Expression

Lambda Expression in Java is a new and important feature introduced in Java SE 8. It provides a clear and concise way to represent a method from an interface using an expression. It is especially useful in the collections library, helping to iterate, filter, and extract data from collections.

A Lambda Expression is used to provide an implementation of a functional interface. It significantly reduces the amount of code. With Lambda expressions, we do not need to redefine a method to provide the implementation; instead, we can directly write the executing code.

A Java Lambda Expression is considered as a function, and thus the compiler does not generate a .class file for it.

1. **Functional Interface**: 

An interface that contains only one abstract method is known as a functional interface. It can have any number of default, static, and private methods. It is also known as a Single Abstract Method (SAM) interface. The `@FunctionalInterface` annotation is used to ensure that the interface is a functional interface.

```java
@FunctionalInterface
interface MyFunctionalInterface {
    void myMethod();
}
```

2. **Syntax**:

The syntax of a Lambda Expression is as follows:
```
(parameters) -> {expression}
```

Why do we need Lambda Expressions?
- To provide the implementation of an interface that contains a single abstract method.
- Requires writing less code.
- Improves code readability and maintainability.

3. **Example**:

Example without Lambda Expression:
```java
public class LambdaExpressionExample {
    public static void main(String args[]) {
        int width = 10;
        MyFunctionalInterface myFunctionalInterface = new MyFunctionalInterface() {
            @Override
            public void myMethod() {
                System.out.println("Width is: " + width);
            }
        };
        myFunctionalInterface.myMethod();
    }
}
```
Output:
```
Width is: 10
```

Example with Lambda Expression:
```java
public class LambdaExpressionExample {
    public static void main(String args[]) {
        int width = 10;
        MyFunctionalInterface myFunctionalInterface = () -> {
            System.out.println("Width is: " + width);
        };
        myFunctionalInterface.myMethod();
    }
}
```
Output:
```
Width is: 10
```

Example with Lambda Expression with  No Parameters:
```java
@FunctionalInterface
interface MyFunctionalInterface {
    void myMethod();
}

public class LambdaExpressionExample {
    public static void main(String args[]) {
        MyFunctionalInterface myFunctionalInterface = () -> {
            System.out.println("Hello, Lambda Expression!");
        };
        myFunctionalInterface.myMethod();
    }
}
```
Output:
```
Hello, Lambda Expression!
```

Example with Lambda Expression with Parameters:
```java
@FunctionalInterface
interface MyFunctionalInterface {
    void myMethod(int a, int b);
}

public class LambdaExpressionExample {
    public static void main(String args[]) {
        MyFunctionalInterface myFunctionalInterface = (a, b) -> {
            System.out.println("Sum: " + (a + b));
        };
        myFunctionalInterface.myMethod(10, 20);
    }
}
```
Output:
```
Sum: 30
```

Example with Lambda Expression without `Return` Keyword:
```java
@FunctionalInterface
interface MyFunctionalInterface {
    void myMethod();
}

public class LambdaExpressionExample {
    public static void main(String args[]) {
        MyFunctionalInterface myFunctionalInterface = () -> System.out.println("Hello, Lambda Expression!");
        myFunctionalInterface.myMethod();
    }
}
```
Output:
```
Hello, Lambda Expression!
```

Example with Lambda Expression with `forEach` Method:
```java
import java.util.ArrayList;
import java.util.List;

public class LambdaExpressionExample {
    public static void main(String args[]) {
        List<String> list = new ArrayList<String>();
        list.add("One");
        list.add("Two");
        list.add("Three");
        list.add("Four");
        list.forEach(
            (n) -> System.out.println(n)
        );
    }
}
```
Output:
```
One
Two
Three
Four
```

Example with Lambda Expression with `Thread`:
```java
package vn.viettuts.java8;

public class LambdaExpressionExample9 {
    public static void main(String[] args) {
        // Thread example without using Lambda Expression
        Runnable r1 = new Runnable() {
            public void run() {
                System.out.println("Thread1 is running...");
            }
        };
        Thread t1 = new Thread(r1);
        t1.start();

        // Thread example using Lambda Expression
        Runnable r2 = () -> {
            System.out.println("Thread2 is running...");
        };
        Thread t2 = new Thread(r2);
        t2.start();
    }
}

```
Output:
```
Thread1 is running...
Thread2 is running...
```

Example with Lambda Expression with `Comparator`:
```java
import java.util.ArrayList;
import java.util.Collections;

public class LambdaExpressionExample {
    public static void main(String args[]) {
        ArrayList<Integer> list = new ArrayList<Integer>();
        list.add(10);
        list.add(0);
        list.add(15);
        list.add(5);
        list.add(20);

        System.out.println("Sorting the list elements...");
        Collections.sort(list, (a, b) -> {
            return a > b ? 1 : a < b ? -1 : 0;
        });
        list.forEach((n) -> System.out.println(n));
    }
}
```
Output:
```
Sorting the list elements...
0
5
10
15
20
```
