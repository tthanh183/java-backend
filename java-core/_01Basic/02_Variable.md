### Variables
0. **Variables Declaration in Java**
- In Java, a variable is the name of a memory location that holds data. There are 3 types of variables in Java:
    - Local variables
    - Instance variables
    - Static variables

> [!WARNING]
> The name must begin with a letter (a-z, A-Z), an underscore (\_), or a dollar sign ($).  
> The variable name cannot contain spaces.  
> From the second character onwards, you can use letters (a-z, A-Z), underscores (\_), dollar signs ($), or digits (0-9).  
> The variable name cannot be the same as any reserved keywords.  
> Java is case-sensitive (uppercase and lowercase letters are considered different).  

- The syntax for declaring a variable is: `DataType varName [ = value] [, varName2] [ = value2]...;

```Java
package vn.com.example;

public class Variable {
    public static float PI = 3.14;                      // This is a static variable
    int n;                                              // This is an instance variable
    public void method() {                              // This is a local variable
        char c = 'c';
    }
}
```
1. **Local Variables in Java**

Local variables are declared within methods, constructors, or blocks.
- Local variables are created inside methods, constructors, or blocks, and they are destroyed when the method, constructor, or block ends.
- You cannot use "access modifiers" when declaring local variables.
- Local variables are stored in the stack memory.
- You must initialize local variables with a default value before using them.

Example 1:
```java
package vn.com.example;

public class Variable {
     
    public void sayHello() {
        int n = 10;                     // This is a local variable
        System.out.println("The value of n is: " + n);
    }
     
    public static void main(String[] args) {
        Variable localVariable = new Variable();
        localVariable.sayHello();
    }
}
```
```
The value of n is: 10
```
Example 2:
```java
package vn.com.example;

public class Variable {
     
    public void sayHello() {
        int n;                 // This is a local variable
        System.out.println("The value of n is: " + n);
    }
     
    public static void main(String[] args) {
        Variable localVariable = new Variable();
        localVariable.sayHello();
    }
}
```
```
Exception in thread "main" java.lang.Error: Unresolved compilation problem: 
    The local variable n may not have been initialized
```
2. **Instance Variables (Global Variables) in Java**

An instance variable is declared within a class, outside of any methods, constructors, or blocks.
- Instance variables are stored in the heap memory.
- An instance variable is created when an object is instantiated using the new keyword, and it is destroyed when the object is garbage collected.
- Instance variables can be used by methods, constructors, blocks, etc., but they must be accessed through a specific object instance.
- You are allowed to use access modifiers when declaring instance variables. By default, instance variables have the "default" access level.
- Instance variables have default values depending on their data type. For example, if the type is int, short, or byte, the default value is 0. If it is double, the default value is 0.0d, and so on. Therefore, you do not need to initialize instance variables before using them.
- Inside the class where the instance variable is declared, you can directly refer to it by name when using it throughout the class.
```java
package vn.com.example;

public class Student {
    // Instance variable "name" of type String, with default value null
    public String name;

    // Instance variable "age" of type Integer, with default value 0
    private int age;

    // Using the "name" variable in a constructor
    public Student(String name) {
        this.name = name;
    }

    // Using the "age" variable in the setAge method
    public void setAge(int age) {
        this.age = age;
    }

    // Method to display student details
    public void showStudent() {
        System.out.println("Name  : " + name);
        System.out.println("Age : " + age);
    }

    public static void main(String[] args) {
        // Creating an object of the Student class
        Student student = new Student("Nguyen Van A");
        student.setAge(21);  // Setting the age
        student.showStudent();  // Displaying student details
    }
}
```
```
Name: Nguyen Van A
Age: 21
```
3. **Static Variables in Java**

A static variable is declared within a class using the static keyword, outside of any methods, constructors, or blocks.
- There will only be a single copy of static variables, no matter how many objects of the class are created.
- Static variables are stored in their own separate static memory.
- Static variables are created when the program starts running and are destroyed only when the program terminates.
- The default value of a static variable depends on the data type, just like instance variables.
- Static variables are accessed through the name of the class that contains them, with the syntax: ClassName.variableName.
- Inside the class, static methods use static variables by referring to them directly is also declared with the static keyword.
```java
package vn.com.example;

public class Student {
    // Static variable 'name'
    public static String name = "Nguyen Van A";
     
    // Static variable 'age'
    public static int age = 21;

    public static void main(String[] args) {
        // Accessing static variable directly
        System.out.println("Name : " + name);

        // Accessing static variable using the class name
        System.out.println("Age : " + Student.age);
    }
}
```
```
Name : Nguyen Van A
Age : 21
```
