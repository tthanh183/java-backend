### For Each Loop

The for-each loop in Java was introduced in Java 5. It is primarily used to iterate over arrays or elements of a collection. The advantage of the for-each loop is that it eliminates the possibility of errors and makes the code more readable.

Syntax:
```
for (type var : array) {
    // code block to be executed
}
```

Example of iterating over an array:
```java
public class ForEachExample {
    public static void main(String[] args) {
        int[] numbers = {1, 2, 3, 4, 5};
        for (int number : numbers) {
            System.out.println(number);
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
5
```

Example of iterating over a collection:
```java
import java.util.ArrayList;
import java.util.List;

public class ForEachExample {
    public static void main(String[] args) {
        List<String> names = new ArrayList<>();
        names.add("Alice");
        names.add("Bob");
        names.add("Charlie");
        for (String name : names) {
            System.out.println(name);
        }
    }
}
```
Output:
```
Alice
Bob
Charlie
```

