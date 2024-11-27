### Varargs (Variable Arguments)

Variable Argument - Varargs in Java allows a method to accept zero or multiple arguments. Before varargs, we used method overloading or passed an array as a method parameter, but these approaches were not ideal. If we don't know how many arguments will be passed to the method, varargs is a better approach.

Varargs uses an ellipsis (...) to denote variable arguments. The syntax of varargs is:
```
returnType methodName(dataType... variableName)
```

Example:
```java

class VarargsExample {
    static void display(String... values) {
        System.out.println("Displaying values:");
        for (String value : values) {
            System.out.println(value);
        }
    }

    public static void main(String[] args) {
        display(); // zero argument
        display("Hello"); // one argument
        display("Hello", "World"); // two arguments
    }
}
```
Output:
```
Displaying values:
Displaying values:
Hello
Displaying values:
Hello
World
```

Principal for using varargs:
- There can be only one variable argument in a method.
- Variable argument (varargs) must be the last argument in the method.

Example of Varargs causing a compile-time error:
```
void method(String... a, int... b) {} // Compile-time error
void method(int... a, String b) {}    // Compile-time error
```

Example of Varargs in the correct position:
```java
void method(int a, String... b) {} // Correct
void method(String... a) {}        // Correct
```
