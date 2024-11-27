### Annotation 

An annotation in Java is a tag that represents metadata, meaning it is attached to a class, interface, method, or field to specify additional information that can be used by the Java compiler and the JVM.

Java annotations are used to provide extra information, so they serve as an alternative to XML and Marker interfaces.

There are two types of Java annotations:
- Built-in annotations in Java.
- Custom annotations - defined by the user.

There are several built-in Java annotations. Some annotations are used in Java code, while others are used in other annotations.

Annotations used in Java code:
- `@Override`
- `@SuppressWarnings`
- `@Deprecated`

Annotations used in other annotations:
- `@Target`
- `@Retention`
- `@Inherited`
- `@Documented`

1. `@Override`:

The @Override annotation ensures that a method overridden in a subclass must match the method in the parent class in name and signature. If it doesn't, a compilation error will occur.

Example:
```java
class Animal {
    void eatSomething() {
        System.out.println("eating something");
    }
}

class Dog extends Animal {
    @Override
    void eatsomething() { // This should be eatSomething (method name is case-sensitive)
        System.out.println("eating foods");
    }
}

class TestAnnotation1 {
    public static void main(String args[]) {
        Animal a = new Dog();
        a.eatSomething();
    }
}

```
Output:
```
Compilation error: method does not override or implement a method from a supertype
```
2. `@SuppressWarnings`:

The @SuppressWarnings annotation is used to suppress compiler warnings. It can be used to prevent warnings from being displayed for specific sections of code.

```java
import java.util.ArrayList;
 
class TestAnnotation2 {
    @SuppressWarnings({ "unchecked", "rawtypes" })
    public static void main(String args[]) {
        ArrayList list = new ArrayList();
        list.add("C++");
        list.add("Java");
        list.add("PHP");
 
        for (Object obj : list)
            System.out.println(obj);
    }
}
```

If you remove @SuppressWarnings({ "unchecked", "rawtypes" }), the compiler will display warnings during compilation because non-generic collections are being used.


3. `@Deprecated`:

The @Deprecated annotation marks a method or class as outdated and not recommended for use. It informs users that the feature may be removed in future versions, so it is better to avoid using such methods or classes.

Example:
```java
class A {
    void m() {
        System.out.println("hello m");
    }
 
    @Deprecated
    void n() {
        System.out.println("hello n");
    }
}
 
class TestAnnotation3 {
    public static void main(String args[]) {
        A a = new A();
        a.n(); // Warning will be shown during compilation
    }
}
```

At the compile time, you will get a warning message: "The method n() from the type A is deprecated."