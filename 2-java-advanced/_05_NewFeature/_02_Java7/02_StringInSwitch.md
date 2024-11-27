### String in switch

In Java 7, Java allows you to use String objects in switch-case statements. To use String, you need to consider the following points:

- It must be a String object.

```
Object game = "Hockey"; // not allowed
String game = "Hockey"; // allowed to use
```

- String objects are case-sensitive.

```
"Hockey" and "hockey" are different.
```

- It cannot be a NULL value. Be cautious when passing a String object, as passing a null object will cause a NullPointerException.

Example:

```java
public class StringInSwitch {
    public static void main(String[] args) {
        String game = "Hockey";
        switch (game) {
            case "Cricket":
                System.out.println("Cricket");
                break;
            case "Hockey":
                System.out.println("Hockey");
                break;
            case "Football":
                System.out.println("Football");
                break;
            default:
                System.out.println("No match found");
        }
    }
}
```
Output:

```
Hockey
```


