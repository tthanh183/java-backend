### Super Keyword in Java
The `super` keyword in Java is a reference variable used to refer to the immediate parent class object. Whenever you create an instance of a subclass, an instance of the superclass is implicitly created and can be referenced by the `super` keyword.

In Java, the `super` keyword has three main uses:
- To refer directly to the instance variable of the parent class.
- To call the constructor of the parent class using `super()`.
- To call a method from the parent class using `super`.

1. **Referencing the Instance Variable of the Parent Class**

The `super` keyword is used to refer directly to an instance variable of the parent class.
Example:
```java
class Animal {
    String color = "White";
}

class Dog extends Animal {
    String color = "Black";

    void display() {
        System.out.println("Dog color: " + color);
        System.out.println("Animal color: " + super.color);
    }

    public static void main(String args[]) {
        Dog d = new Dog();
        d.display();
    }
}
```
Output:
```
Dog color: Black
Animal color: White
```
2. **Using `super()` to Call the Parent Class Constructor**
   
In Java, `super()` is used to directly call the constructor of the parent class.
Example:
```java
class Animal {
    Animal() {
        System.out.println("Animal is created");
    }
}

class Dog extends Animal {
    Dog() {
        super();
        System.out.println("Dog is created");
    }

    public static void main(String args[]) {
        Dog d = new Dog();
    }
}
```
Output:
```
Animal is created
Dog is created
```
> [!NOTE]
> `super()` is automatically added by the compiler to each constructor of a class. If you create a constructor and do not have `this()` or `super(`) as the first statement, the compiler will add super() to the first line.
Example:
```java
class Animal {
    Animal() {
        System.out.println("Animal is created");
    }
}

class Dog extends Animal {
    Dog() {
        System.out.println("Dog is created");
    }

    public static void main(String args[]) {
        Dog d = new Dog();
    }
}
```
Output:
```
Animal is created
Dog is created
```
3. **Using `super` to Call a Method from the Parent Class**

The `super` keyword can also be used to call a method from the parent class. This is useful when the subclass has methods with the same name as those in the parent class, as shown in the following example:
```java
class Animal {
    void eat() {
        System.out.println("Animal is eating...");
    }
}

class Dog extends Animal {
    void eat() {
        System.out.println("Dog is eating...");
    }
    
    void bark() {
        super.eat();
    }
    
    public static void main(String args[]) {
        Dog d = new Dog();
        d.bark();
    }
}
```
Output:
``` 
Animal is eating...
```
