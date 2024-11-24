### Object-Oriented Programming (OOP)
OOP is a programming technique that maps real-world objects into a programming language, helps increase productivity, simplify maintenance, and facilitate scalability in software design by providing several key concepts such as:
- Class
- Object
- Inheritance
- Polymorphism
- Encapsulation
- Abstraction

1. **Object**

An entity with state and behavior is called an object.

An object has three characteristics:
- State: Represents the data (values) of an object.
- Behavior: Represents the behavior (functions) of an object, such as depositing money, withdrawing money, etc.
- Identity: The identity of an object is typically implemented through a unique ID. This ID is hidden from the external user, but it is used internally by the JVM to identify each object.

> [!Note]
> An object is an instance of a class.
2. **Class**

A class is a group of objects that share common attributes. It is a blueprint or design from which objects are created.

A class in Java can contain:
- Data members
- Constructor
- Methods
- Blocks
- Classes and Interfaces

Example:
```java
public class Student2 {
    int id;
    String name;

    // insertRecord method
    void insertRecord(int id, String name) {
        this.id = id;
        this.name = name;
    }

    // displayInformation method
    void displayInformation() {
        System.out.println(id + " " + name);
    }

    public static void main(String args[]) {
        Student2 s1 = new Student2();
        Student2 s2 = new Student2();

        s1.insertRecord(111, "Tran");
        s2.insertRecord(222, "Thanh");

        s1.displayInformation();
        s2.displayInformation();
    }
}

```
Output:
```
111 Tran
222 Thanh
```
3. **Ways to Create an Object in Java**

There are several ways to create objects in Java:
- By `new` keyword
- By `newInstance()` method
- By `clone()` method
- By deserialization, etc.
4. **Anonymous Objects in Java**
`Anonymous` means without a name. An object without a reference is called an anonymous object.
If you use an object only once, creating an anonymous object is the best choice in this case.
Example:
```java
public class Calculation {

    void fact(int n) {
        int factorial = 1;
        for (int i = 1; i <= n; i++) {
            factorial = factorial * i;
        }
        System.out.println("Factorial of " + n + " is: " + factorial);
    }

    public static void main(String args[]) {
        // calling method of anonymous object
        new Calculation().fact(5);
    }
}
```
Output:
```
Factorial of 5 is: 120
```
4. **Difference between Object and Class**

| Class                                                                | Object                                                               |
|----------------------------------------------------------------------|----------------------------------------------------------------------|
| A class is a blueprint or template from which objects are created.   | An object is an instance of a class.                                 |
| A class is a logical entity.                                         | An object is a real-world entity.                                    |
| A class is declared only once.                                       | An object is created many times as per the requirement.              |
| A class is declared using the `class` keyword.                       | An object is created using the `new` keyword.                        |
| A class is created in the method area.                               | An object is created in the heap memory.                             |
| A class is used to define the properties and behaviors of an object. | An object is used to access the properties and behaviors of a class. |
| A class is used to define the methods of an object.                  | An object is used to call the methods of a class.                    |
| A class is used to define the data members of an object.             | An object is used to access the data members of a class.             |
| A class is used to define the static members of an object.           | An object is used to access the non-static members of a class.       |




















