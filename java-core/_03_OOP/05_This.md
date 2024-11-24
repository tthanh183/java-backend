### `This` keyword in Java
The `this` keyword in Java is a reference variable used to refer to the current object's instance.
In Java, `this` keyword is used in six different ways:
- To refer to the instance variable of the current class.
- `this()`can be used to call the constructor of the current class.
- The `this` keyword can be used to call methods of the current class.
- The `this` keyword can be passed as a parameter in methods.
- The `this` keyword can be passed as a parameter in constructors.
- The `this` keyword can be used to return the instance of the current class.

1. **To refer to the instance variable of the current class**

The `this` keyword in Java can be used to refer to the instance variable of the current class. This is particularly useful when there is a name conflict between the instance variable and the parameter in a method or constructor.
```java
public class Student {
    int id;
    String name;

    Student(int id, String name) {
        id = id;           
        name = name;       
    }

    void display() {
        System.out.println(id + " " + name);
    }

    public static void main(String args[]) {
        Student s1 = new Student(111, "Karan");
        Student s2 = new Student(222, "Aryan");
        s1.display();
        s2.display();
    }
}
```
Output:
```
0 null
0 null
```
In the example above, the parameter names in the constructor are the same as the instance variable names, so the instance variables were not assigned correctly. To fix this issue, we need to use the `this` keyword to distinguish between the local variable and the instance variable.
```java
public class Student {
    int id;
    String name;

    Student(int id, String name) {
        this.id = id;           
        this.name = name;       
    }

    void display() {
        System.out.println(id + " " + name);
    }

    public static void main(String args[]) {
        Student s1 = new Student(111, "Karan");
        Student s2 = new Student(222, "Aryan");
        s1.display();
        s2.display();
    }
}
```
Output:
```
111 Karan
222 Aryan
```
If the local variable and the instance variable have different names, there's no need to use `this` keyword.

2. **`this()` can be used to call the constructor of the current class**

The `this()` method can be used to call a constructor of the current class. This is useful when you have multiple constructors in a class and you want to reuse one constructor within another.
```java
public class Student {
    int id;
    String name;

    Student() {
        System.out.println("Default constructor is invoked");
    }

    Student(int id, String name) {
        this(); // calling default constructor
        this.id = id;
        this.name = name;
    }

    void display() {
        System.out.println(id + " " + name);
    }

    public static void main(String args[]) {
        Student s1 = new Student(111, "Karan");
        Student s2 = new Student(222, "Aryan");
        s1.display();
        s2.display();
    }
}
```
Output:
```
Default constructor is invoked
Default constructor is invoked
111 Karan
222 Aryan
```
You can use `this()` to reuse a constructor within another constructor. This maintains a chain of constructors.
Example:
```java
public class Student {
    int id;
    String name;
    String city;

    Student(int id, String name) {
        this.id = id;
        this.name = name;
    }

    Student(int id, String name, String city) {
        this(id, name); // reusing constructor
        this.city = city;
    }

    void display() {
        System.out.println(id + " " + name + " " + city);
    }

    public static void main(String args[]) {
        Student s1 = new Student(111, "Karan", "Delhi");
        Student s2 = new Student(222, "Aryan", "Mumbai");
        s1.display();
        s2.display();
    }
}
```
Output:
```
111 Karan Delhi
222 Aryan Mumbai
```
> [!NOTE]
> The `this()` method must be the first statement in the constructor.
3. **Calling methods of the current classs**

You can use the this keyword to call methods of the current class. If you don't use this, the compiler will automatically add the this keyword when calling the method.
```java
public class Student {
    void display() {
        System.out.println("Method is invoked");
    }

    void show() {
        this.display(); // calling display method
    }

    public static void main(String args[]) {
        Student s1 = new Student();
        s1.show();
    }
}
```
Output:
```
Method is invoked
```
4. **Using `this` as a parameter in a method.**

The `this` keyword can be used as a parameter in a method. This usage is primarily seen in event handling scenarios.
```java
public class Student {
    void m(Student obj) {
        System.out.println("Method is invoked");
    }

    void p() {
        m(this);
    }

    public static void main(String args[]) {
        Student s1 = new Student();
        s1.p();
    }
}
```
Output:
```
Method is invoked
```
The `this` keyword is used as a parameter in event handling or in situations where we need to pass a reference of the current class to another class.
5. ** Using this as a parameter in a Constructor**
  
You can also pass `this` as a parameter in a constructor. This feature is useful when we need to use an object in multiple classes.
```java
class A {
    int data = 10;

    A() {
        Student s = new Student(this);
        s.display();
    }
}
public class Student {
    A obj;

    Student(A obj) {
        this.obj = obj;
    }

    void display() {
        System.out.println(obj.data);
    }

    public static void main(String args[]) {
        A a = new A();
        Student s1 = new Student(a);
        s1.display();
    }
}
```
Output:
```
10
```
6. **Using this to return the instance of the current class**
   
We can return the instance of the current class using the `this` keyword. In this case, the return type of the method must be of the class type (not a primitive type).
```java
public class Student {
    Student get() {
        return this;
    }

    void display() {
        System.out.println("Method is invoked");
    }

    public static void main(String args[]) {
        Student s1 = new Student();
        s1.get().display();
    }
}
```
Output:
```
Method is invoked
```