### Access Modifier in Java
Access Modifiers in Java define the visibility and accessibility of variables, methods, constructors, or classes. There are 4 types of Access Modifiers in Java:

- private
- default (default)
- protected
- public

1. **Overview**

| Modifier  | Class | Package | Subclass | World |
|-----------|-------|---------|----------|-------|
| public    | Y     | Y       | Y        | Y     |
| protected | Y     | Y       | Y        | N     |
| default   | Y     | Y       | N        | N     |
| private   | Y     | N       | N        | N     |

2. **Private Access Modifier**

The private access modifier restricts access to within the same class. This means that any members declared as private cannot be accessed from outside the class.

Example:
```java
class A {
    private int data = 40;

    private void msg() {
        System.out.println("Hello Java");
    }
}

public class Simple {
    public static void main(String args[]) {
        A obj = new A();
        System.out.println(obj.data); // Compile Time Error
        obj.msg(); // Compile Time Error
    }
}
```
Output:
```
Compile Time Error
Compile Time Error
```
