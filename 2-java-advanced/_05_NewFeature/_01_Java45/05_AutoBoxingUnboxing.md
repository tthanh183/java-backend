### Autoboxing and Unboxing

Autoboxing and unboxing are features introduced in Java 5. They automatically convert primitive data types into their corresponding wrapper classes and vice versa. Autoboxing refers to converting a primitive type into its wrapper class, while unboxing is the reverse process. This feature allows Java developers to work seamlessly with primitive types and wrapper classes without writing explicit conversion code.

Advantages of autoboxing and unboxing: There's no need to manually convert between primitives and wrapper classes.

Example of autoboxing:
```java
public class AutoboxingExample {
    public static void main(String[] args) {
        Integer num = 10; // autoboxing
        System.out.println(num);
    }
}
```
Output:
```
10
```

Example of unboxing:
```java
public class UnboxingExample {
    public static void main(String[] args) {
        Integer num = 10;
        int value = num; // unboxing
        System.out.println(value);
    }
}
```
Output:
```
10
```

Autoboxing and unboxing with comparison:
```java
public class UnboxingExample2 {
    public static void main(String args[]) {
        Integer i = new Integer(50);
        if (i < 100) { // unboxing
            System.out.println(i);
        }
    }
}
```
Output:
```
50
```

Autoboxing and unboxing with method overloading:

> [!NOTE]
> When a method is overloaded, widening beats boxing, boxing beats var-args, and var-args beats widening.


