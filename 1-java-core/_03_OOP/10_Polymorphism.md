### Polymorphism
Polymorphism in Java is a concept where we can perform an action in multiple ways.
There are two types of polymorphism in Java: compile-time polymorphism and runtime polymorphism. 

Runtime polymorphism is the process where the overridden method is called at runtime. During this process, an overridden method is invoked through a reference variable of the parent class.

Before diving into runtime polymorphism, let's first understand Upcasting.

1. **Upcasting**

Upcasting occurs when a parent class reference variable points to an object of a child class. Example:
```
class A {}
class B extends A {}

A a = new B(); // Upcasting
}
```
Example 1:
```java
class Animal {
    void eat() {
        System.out.println("eating...");
    }
}

class Dog extends Animal {
    void eat() {
        System.out.println("eating bread...");
    }
}

class TestPolymorphism2 {
    public static void main(String args[]) {
        Animal a;
        a = new Dog();
        a.eat();
    }
}
```
Output:
```
eating bread...
```
Example 2:
```java
class Animal {
    void eat() {
        System.out.println("eating...");
    }
}

class Dog extends Animal {
    void eat() {
        System.out.println("eating bread...");
    }
}

class Cat extends Animal {
    void eat() {
        System.out.println("eating rat...");
    }
}

class TestPolymorphism3 {
    public static void main(String args[]) {
        Animal a;
        a = new Dog();
        a.eat();
        a = new Cat();
        a.eat();
    }
}
```
Output:
```
eating bread...
eating rat...
```
2. **Runtime Polymorphism with Data Members in Java**

A method can be overridden, but data members cannot be overridden.
Example:
```java
class Animal {
    String color = "white";
}

class Dog extends Animal {
    String color = "black";

    public static void main(String args[]) {
        Animal a = new Dog();
        System.out.println(a.color);            // white
    }
}
```
Output:
```
white
```
> [!NOTE]
> Data members cannot be overridden in Java. They are hidden in the child class.
3. **Runtime Polymorphism with Multilevel Inheritance in Java**

```java
class Animal {
    void eat() {
        System.out.println("eating...");
    }
}

class Dog extends Animal {
    void eat() {
        System.out.println("eating bread...");
    }
}

class BabyDog extends Dog {
    void eat() {
        System.out.println("drinking milk...");
    }
    
    public static void main(String args[]) {
        Animal a = new BabyDog();
        Animal b = new Dog();
        Animal c = new Animal();
        a.eat();
        b.eat();
        c.eat();
    }
}
```
Output:
```
drinking milk...
eating bread...
eating...
```
> [!NOTE]
> If BabbyDog does not have an `eat()` method, the `eat()` method of the parent class Dog will be called.

