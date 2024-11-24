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
> [!NOTE]
>  A class itself cannot be declared as private or protected, except for nested classes.

3. **Default (no modifier) Access Modifier**

If no access modifier is specified, it is considered the default access modifier. The default access modifier allows access only within the same package.

Example:
```java
package pack;
class A {
    void msg() {
        System.out.println("Hello");
    }
}
```
```java
package mypack;
import pack.*;
public class B {
    public static void main(String args[]) {
        A obj = new A();            // Compile Time Error
        obj.msg();                  // Compile Time Error
    }
}
```


4. **Protected Access Modifier**

The protected access modifier allows access within the same package and also outside the package, but only if inheritance is involved. It can be applied to variables, methods, and constructors but not to classes.
Example:
```java
package pack;
protected class A {
    protected void msg() {
        System.out.println("Hello");
    }
}
```
```java
package mypack;
import pack.*;
public class B extends A {
    public static void main(String args[]) {
        B obj = new B();
        obj.msg();
    }
}
```
Output:
```
Hello
```
5. **Public Access Modifier**

The public access modifier allows access from anywhere, without any restrictions on package.
Example:
```java
package pack;
public class A {
    public void msg() {
        System.out.println("Hello");
    }
}
```
```java
package mypack;
import pack.*;
public class B {
    public static void main(String args[]) {
        A obj = new A();
        obj.msg();
    }
}
```
Output:
```
Hello
```