### Custom Exception in Java

If you create your own exception, it is known as a custom exception or user-defined exception. Custom exceptions in Java are used to tailor exceptions to the user's needs.
```java
class CustomException extends Exception {
    CustomException(String s) {
        super(s);
    }
}
```

Example:
```java
public class InvalidAgeException extends Exception {
    InvalidAgeException(String s) {
        super(s);
    }
}

class TestCustomException {
    static void validate(int age) throws InvalidAgeException {
        if (age < 18) {
            throw new InvalidAgeException("not valid");
        } else {
            System.out.println("welcome to vote");
        }
    }

    public static void main(String[] args) {
        try {
            validate(13);
        } catch (InvalidAgeException e) {
            System.out.println(e);
        }
        System.out.println("rest of the code");
    }
}
```
Output:
```
InvalidAgeException: not valid
rest of the code
```