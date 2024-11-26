### Interface in Java

An interface in Java is a completely abstract class that is used to group related methods with empty bodies. The interface is used to achieve abstraction and multiple inheritance in Java.
It represents the HAS-A relationship. An interface cannot be instantiated.

Java Compiler automatically adds the `public` and `abstract` keywords before the methods of an interface, and the `public`, `static`, and `final` keywords before data members. 
In other words, fields in an Interface are `public`, `static`, and `final` by default, and methods are `public` and `abstract`.

Unless a class implementing an interface is abstract, all methods of the interface need to be defined in the class implementing it.
```
interface Animal {
    void sound();
    void eat();
}

class Dog implements Animal {
    public void sound() {
        System.out.println("Barking...");
    }
    public void eat() {
        System.out.println("Eating...");
    }
}
```

Rules for Overriding Methods Defined in Interface:
- The signature of the interface method and the return type should be maintained while overriding the method.
- Checked exceptions should not be declared in the `implements` method. Instead, they should be declared in the interface method or in subclasses declared by the interface method.
- The overriding method cannot be more restrictive than the abstract method. For example, if the interface method is declared as `public`, the overriding method in the class cannot be `private`.

Rules for Implementing Interfaces:
- A class can extend only one class, but it can implement multiple interfaces.
- An interface can extend multiple interfaces.

Example1:
```java
interface Animal {
    void sound();
    void eat();
}

interface Pet {
    void play();
}

class Dog implements Animal, Pet {
    public void sound() {
        System.out.println("Barking...");
    }
    public void eat() {
        System.out.println("Eating...");
    }
    public void play() {
        System.out.println("Playing...");
    }
}

class TestInterface {
    public static void main(String[] args) {
        Dog dog = new Dog();
        dog.sound();
        dog.eat();
        dog.play();
    }
}
```
Output:
```
Barking...
Eating...
Playing...
```

Example2: 
```java
interface Animal {
    void sound();
    void eat();
}

interface Bird extends Animal {
    void fly();
}

class Parrot implements Bird {
    public void sound() {
        System.out.println("Chirping...");
    }
    public void eat() {
        System.out.println("Eating...");
    }
    public void fly() {
        System.out.println("Flying...");
    }
}
```
Output:
```
Chirping...
Eating...
Flying...
```

> [!NOTE]
> In Java 8, default and static methods were introduced in interfaces.
> - Default methods: These methods have a default implementation in the interface itself. They can be overridden in the implementing class.
> - Static methods: These methods are similar to static methods in classes. They can be called without creating an instance of the interface.

Example:
```java
interface Animal {
    void sound();
    void eat();
    default void sleep() {
        System.out.println("Sleeping...");
    }
    static void walk() {
        System.out.println("Walking...");
    }
}

class Dog implements Animal {
    public void sound() {
        System.out.println("Barking...");
    }
    public void eat() {
        System.out.println("Eating...");
    }
}

class TestInterface {
    public static void main(String[] args) {
        Dog dog = new Dog();
        dog.sound();
        dog.eat();
        dog.sleep();
        Animal.walk();
    }
}
```
Output:
```
Barking...
Eating...
Sleeping...
Walking...
```