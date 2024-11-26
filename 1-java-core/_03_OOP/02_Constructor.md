### Constructor in Java

1. **Constructor**

Constructor  is a special method that is used to initialize an object. It is called when an object of a class is created.  

2. **Rules for Creating Constructors in Java**

There are 2 basic rules for creating constructors:
- The constructor name must be the same as the name of the class it is in.
- A constructor does not have a return type.
3. **Types of Constructors in Java**

There are two types of constructors in Java:
- Default constructor: A constructor that has no parameter is known as default constructor.
```
<class_name>() {
    // code
}
```
Example:
```java
class Bike {  
    Bike() {
        System.out.println("Bike is created");
    }  
    public static void main(String args[]) {  
        Bike b = new Bike();  
    }  
}
```
Output:
```
Bike is created
```
> [!NOTE]
> If there is no constructor in a class, the compiler automatically creates a default constructor for that class. The default constructor is used to provide the default values to the object like 0, null, etc., depending on the type.
- Parameterized constructor: A constructor that has parameters is known as a parameterized constructor. A parameterized constructor is used to provide different values for different objects.
Example:
```java
class Bike {  
    int id;  
    String name;  
    Bike(int i, String n) {
        id = i;
        name = n;
    }  
    void display() {
        System.out.println(id + " " + name);
    }  
    public static void main(String args[]) {  
        Bike b1 = new Bike(111, "KTM");  
        Bike b2 = new Bike(222, "Duke");  
        b1.display();  
        b2.display();  
    }  
}
```
Output:
```
111 KTM
222 Duke
```
4. **Constructor Overloading in Java**

Constructor Overloading is a technique in Java. You can create multiple constructors in the same class with different parameter lists. The compiler differentiates these constructors based on the number and type of parameters passed.  
Example:
```java
class Student {  
    int id;  
    String name;  
    int age;  
    Student(int i, String n) {
        id = i;
        name = n;
    }  
    Student(int i, String n, int a) {
        id = i;
        name = n;
        age = a;
    }  
    void display() {
        System.out.println(id + " " + name + " " + age);
    }  
    public static void main(String args[]) {  
        Student s1 = new Student(111, "Karan");  
        Student s2 = new Student(222, "Aryan", 25);  
        s1.display();  
        s2.display();  
    }  
}
```
Output:
```
111 Karan 0
222 Aryan 25
```
5. **Difference between constructor and method**

| Constructor                                 | Method                                         |    
|---------------------------------------------|------------------------------------------------|
| Constructor is used to initialize an object | Method is used to expose behavior of an object |
| Constructor does not have a return type     | Method has a return type                       |
| Constructor is invoked implicitly           | Method is invoked explicitly                   |
    
    
