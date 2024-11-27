### Anonymous Classes

In Java, an Anonymous Class is a class that is defined and instantiated in a single statement, without giving it a name. It is often used when you need to create a one-time use class to implement an interface or extend a class. These classes are called "anonymous" because they do not have a name and are declared and used in place in the code.

Anonymous classes are frequently used for:
- Implementing interfaces with a single method (Functional Interfaces).
- Extending classes that are not intended to be reused.
- Providing functionality for one-time use, often for event handling in GUIs, thread management, and callbacks.

Key features of Anonymous Classes:  
- No name: The class is unnamed and declared inline where it is used.
- Local declaration: They are typically declared within a method, constructor, or block of code, which makes them a form of local class.
- Implementation of interfaces or extension of classes: Anonymous classes are used to instantiate classes that implement interfaces or extend existing classes (abstract or concrete).
- Access to variables: Anonymous classes can access final (or effectively final) local variables and instance variables from their enclosing scope.


Example of an Anonymous Class implementing an interface:
```java
interface MyInterface {
    void myMethod();
}

public class AnonymousClassExample {
    public static void main(String[] args) {
        MyInterface myInterface = new MyInterface() {
            @Override
            public void myMethod() {
                System.out.println("Implementation of myMethod");
            }
        };
        myInterface.myMethod();
    }
}
```

Example of an Anonymous Class extending a class:
```java
class MySuperClass {
    void display() {
        System.out.println("Display method of superclass");
    }
}
    
public class AnonymousClassExample {
    public static void main(String[] args) {
        MySuperClass mySuperClass = new MySuperClass() {
            @Override
            void display() {
                System.out.println("Display method of anonymous class");
            }
        };
        mySuperClass.display();
    }
}
```
