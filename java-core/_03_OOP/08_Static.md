### `Static` Keyword in Java
The `static` keyword in Java is primarily used for memory management. It can be applied to variables, methods, blocks, and nested classes. The static keyword belongs to the class, not to instances of the class.

In Java, static can refer to:
- Static variables: These are variables that belong to the class rather than an instance of the class.
- Static methods: These are methods that can be called on the class itself, not requiring an instance.
- Static blocks: These are used for initializing static data members.

1. **Static Variables**

When you declare a variable as static, it is called a `static` variable.  
- A static variable can be used to refer to properties that are common across all objects of a class (rather than being unique to each object).
- A static variable occupies memory only once in the class area at the time the class is loaded.

> [!TIP]
> Using `static` variables helps your program utilize memory more efficiently (saving memory), as the variable is shared among all objects of the class.

Example:
```java

class Student {
    int rollno;
    String name;
    static String college = "ITS";

    Student(int r, String n) {
        rollno = r;
        name = n;
    }

    void display() {
        System.out.println(rollno + " " + name + " " + college);
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
111 Karan ITS
222 Aryan ITS
```
2. **Static Methods**

When you declare a method as static, it is called a `static` method.
- A static method belongs to the class, not to any instance of the class.
- A static method can be called without creating an instance of the class.
- A static method can access static variables and change their values.

Example:
```java

class Student {
    int rollno;
    String name;
    static String college = "ITS";

    Student(int r, String n) {
        rollno = r;
        name = n;
    }

    static void change() {
        college = "BBDIT";
    }

    void display() {
        System.out.println(rollno + " " + name + " " + college);
    }

    public static void main(String args[]) {
        Student.change();

        Student s1 = new Student(111, "Karan");
        Student s2 = new Student(222, "Aryan");

        s1.display();
        s2.display();
    }
}
```
Output:
```
111 Karan BBDIT
222 Aryan BBDIT
```
> [!NOTE]
> There are two main limitations of `static` methods:
> - A `static` method cannot use non-static variables or call non-static methods directly.
> - The `this` and `super` keywords cannot be used in a static context.
3. **Static Blocks**

Static blocks are used to initialize static data members. They are executed before the main method when the class is loaded.
Example:
```java

class Student {
    int rollno;
    String name;
    static String college;

    static {
        college = "BBDIT";
    }

    Student(int r, String n) {
        rollno = r;
        name = n;
    }

    void display() {
        System.out.println(rollno + " " + name + " " + college);
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
111 Karan BBDIT
222 Aryan BBDIT
```

### Frequently Asked Questions
Question: Why is the main method in Java static?  
Answer: The main method is static because it is called by the Java Virtual Machine (JVM) before any objects are created. If the main method were non-static, the JVM would need to create an instance of the class first, which could lead to memory allocation issues.
Question: Can we override a static method in Java?
Answer: No, you cannot override a static method in Java. However, you can create a method with the same signature in the subclass, which is called method hiding.
Question: Can we run a Java program without a main method?
Answer: Yes, one way to do this is using a static block in earlier versions of the JDK (before JDK 1.7). However, from JDK 1.7 onward, the JVM requires a main method to start execution.