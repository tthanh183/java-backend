### Inheritance
1. **Inheritance in Java**

Inheritance in Java is a relationship between two classes where one class (the subclass) inherits the properties and methods of another class (the superclass). Through inheritance, the subclass inherits all the public and protected members of the superclass, but it cannot access the private members of the superclass.   
The concept of inheritance in Java allows the creation of a new class based on an existing class, enabling the reuse of methods and attributes of the superclass while adding new methods or attributes.
```
class Subclass-name extends Superclass-name {
   // methods and fields
}
```
Example:
```java
class Animal {
    void eat() {
        System.out.println("eating...");
    }
}

class Dog extends Animal {
    void bark() {
        System.out.println("barking...");
    }
}

public class TestInheritance {
    public static void main(String args[]) {
        Dog d = new Dog();
        d.bark();
        d.eat();
    }
}
```
Output:
```
barking...
eating...
```
2. **Types of Inheritance in Java**

There are three types of inheritance in Java: single inheritance, multilevel inheritance, and hierarchical inheritance.
- Single inheritance: When a class inherits from one class only.
- Multilevel inheritance: When a class is derived from another class, which is itself derived from another class.
- Hierarchical inheritance: When multiple classes inherit from a single parent class.
> [!Warning]
> Java does not support multiple inheritance through classes, but it allows multiple inheritance through interfaces.
Example1: Single inheritance
```java
class Animal {
    void eat() {
        System.out.println("eating...");
    }
}

class Dog extends Animal {
    void bark() {
        System.out.println("barking...");
    }
}

public class TestInheritance {
    public static void main(String args[]) {
        Dog d = new Dog();
        d.bark();
        d.eat();
    }
}
```
Output:
```
barking...
eating...
```
Example2: Multilevel inheritance
```java
class Animal {
    void eat() {
        System.out.println("eating...");
    }
}
    
class Dog extends Animal {
    void bark() {
        System.out.println("barking...");
    }
}

class BabyDog extends Dog {
    void weep() {
        System.out.println("weeping...");
    }
}

public class TestInheritance {
    public static void main(String args[]) {
        BabyDog bd = new BabyDog();
        bd.weep();
        bd.bark();
        bd.eat();
    }
}
```
Output:
```
weeping...
barking...
eating...
```
Example3: Hierarchical inheritance
```java
class Animal {
    void eat() {
        System.out.println("eating...");
    }
}
    
class Dog extends Animal {
    void bark() {
        System.out.println("barking...");
    }
}

class Cat extends Animal {
    void meow() {
        System.out.println("meowing...");
    }
}

public class TestInheritance {
    public static void main(String args[]) {
        Cat c = new Cat();
        c.meow();
        c.eat();
    }
}
```
Output:
```
meowing...
eating...
```

Questions: Why Multiple Inheritance is Not Supported in Java?  
To reduce complexity and simplify the language, Java does not support multiple inheritance through classes.  
Consider a scenario with three classes A, B, and C, where C inherits from both A and B. If A and B have methods with the same name, it would be unclear which method the subclass C should invoke. This ambiguity can lead to confusion and errors during runtime.  
Therefore, it is better to catch such errors at compile-time rather than at runtime. Java will give a compile-time error if you attempt to inherit from two classes.

3. **Object Class in Java** 

The Object class is the root class of Java. Every class in Java is a subclass of the Object class. If a class does not extend any other class, it is implicitly a subclass of the Object class. The Object class provides several methods that are common to all classes in Java, such as `toString()`, `equals()`, `hashCode()`, and `getClass()`.
