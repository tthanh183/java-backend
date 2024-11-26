### Abstract Class in Java

A class declared with the abstract keyword is called an abstract class in Java. An abstract class can have both abstract methods and non-abstract methods.

Before understanding abstract classes in Java, you should first understand the concept of abstraction in Java.

1. **Abstraction in Java**

Abstraction is a process of hiding the implementation details and showing only the functionality to the user.
In other words, it only shows the essential features to the user and hides the internal workings. For example, to send a message, the user just types the text and sends it. The internal process of message distribution is hidden.
Abstraction helps you focus more on the object rather than worrying about how it works.

Ways to achieve abstraction in Java:
- Abstract class (0 to 100%)
- Interface (100%)
2. **Abstract Methods in Java**

An abstract method is a method declared with the abstract keyword that does not have an implementation.

If you want a class to have a specific method but the actual implementation of that method is to be determined by subclasses, you can declare the method as abstract in the parent class.
```
// Declaring a method with the abstract keyword and no method body
abstract void printStatus();
```
3. **Abstract Class in Java**

An abstract class is a class declared with the abstract keyword. It can have both abstract methods and non-abstract methods.
```
abstract class Animal {
    abstract void sound();
    void eat() {
        System.out.println("Eating...");
    }
}
```

Example:
```
abstract class Animal {
    abstract void sound();
    void eat() {
        System.out.println("Eating...");
    }
}

class Dog extends Animal {
    void sound() {
        System.out.println("Barking...");
    }
}

class TestAbstraction {
    public static void main(String[] args) {
        Animal animal = new Dog();
        animal.sound();
        animal.eat();
    }
}
```
Output:
```
Barking...
Eating...
```

> [!NOTE]
> In an abstract class, we can have: abstract methods, non-abstract methods, constructors, and instance variables.