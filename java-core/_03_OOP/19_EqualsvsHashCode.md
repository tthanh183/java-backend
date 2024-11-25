### Equals vs HashCode in Java

When working with collections, to achieve the desired behavior, we should override the `equals()`and `hashCode()` methods in the classes of the elements added to the collection.

The Object class (the parent class of all classes in Java) defines two methods: `equals()` and `hashCode()`. This means that all classes in Java (including the ones you create) inherit these methods. Essentially, the Object class provides general-purpose implementations for these methods.

However, you will need to override them specifically for classes whose objects are added to collections, especially collections based on hash tables like `HashSet` and `HashMap`.

1. **The `equals()` Method in Java**

When comparing two objects, Java calls the `equals()` method to return true if the two objects are equal, or false if they are different. Note that comparing objects using the `equals()` method is different from using the == operator.

> [!IMPORTANT]  
> The `equals()` method is designed to compare two objects semantically (by comparing the class's data members), while the == operator compares two objects technically (by comparing their references, i.e., memory addresses).

> [!NOTE]
> The default implementation of the `equals()` method in the Object class compares the references of two objects. This means you should override it in your classes to perform semantic comparisons. Most classes in the JDK override their own equals() method, such as String, Date, Integer, Double, etc.

Example1: The `equals()` Method in Java
```java
public class Example {
    public static void main(String[] args) {
        String s1 = new String("Hello");
        String s2 = new String("Hello");
        System.out.println(s1.equals(s2)); // true
        System.out.println(s1 == s2); // false
    }
}

```
Example2: Overriding the `equals()` Method in Java
```java

public class Employee {
    private String name;
    private int id;

    public Employee(String name, int id) {
        this.name = name;
        this.id = id;
    }

    @Override
    public boolean equals(Object obj) {
        if (this == obj) {
            return true;
        }
        if (obj == null || getClass() != obj.getClass()) {
            return false;
        }
        Employee employee = (Employee) obj;
        return id == employee.id && Objects.equals(name, employee.name);
    }

    @Override
    public int hashCode() {
        return Objects.hash(name, id);
    }

    public static void main(String[] args) {
        Employee emp1 = new Employee("John", 101);
        Employee emp2 = new Employee("John", 101);
        System.out.println(emp1.equals(emp2)); // true
        System.out.println(emp1 == emp2); // false
    }
}
```
Example3: Overriding the `equals()` Method in Java, using `Collections`
```java
import java.util.HashSet;
import java.util.Set;

public class Employee {
    private String name;
    private int id;

    public Employee(String name, int id) {
        this.name = name;
        this.id = id;
    }

    @Override
    public boolean equals(Object obj) {
        if (this == obj) {
            return true;
        }
        if (obj == null || getClass() != obj.getClass()) {
            return false;
        }
        Employee employee = (Employee) obj;
        return id == employee.id && Objects.equals(name, employee.name);
    }

    @Override
    public int hashCode() {
        return Objects.hash(name, id);
    }

    public static void main(String[] args) {
        Employee emp1 = new Employee("John", 101);
        Employee emp2 = new Employee("John", 101);

        Set<Employee> employees = new HashSet<>();
        employees.add(emp1);
        employees.add(emp2);

        System.out.println(employees.size()); // 1
    }
}
```
2. **The `hashCode()` Method in Java**

This hash value is used by hash-based collections like `Hashtable`, `HashSet`, and `HashMap` to store objects in small containers called "buckets." Each bucket is associated with a hash code, and each bucket contains objects with the same hash code.

In other words, a hash table groups its elements by their hash codes. This grouping helps the hash table quickly and efficiently locate an element by searching only within the small group rather than the entire collection.

To find an element in a hash table, the following steps occur:
- The hash table calculates the hash code of the element to find.
- The hash table uses the hash code to locate the bucket that contains the element.
- Inside the bucket, the exact element is found by comparing the specified element with all other elements in that bucket using the equals() method.

Rules for `equals()` and `hashCode()` Methods in Java:
- When overriding the `equals()` method, you must also override the `hashCode()` method.
- If two objects are equal according to the `equals()` method, they must have the same hash code.
- If two objects have the same hash code, they may or may not be equal according to the `equals()` method.

Example: 
```java
import java.util.HashSet;
import java.util.Set;

public class Student {
    private String name;
    private int id;

    public Student(String name, int id) {
        this.name = name;
        this.id = id;
    }

    @Override
    public boolean equals(Object obj) {
        if (this == obj) {
            return true;
        }
        if (obj == null || getClass() != obj.getClass()) {
            return false;
        }
        Student student = (Student) obj;
        return id == student.id && Objects.equals(name, student.name);
    }
    
    public static void main(String[] args) {
        Student student1 = new Student("John", 101);
        Student student2 = new Student("John", 101);

        Set<Student> students = new HashSet<>();
        students.add(student1);
        students.add(student2);

        System.out.println(students.size()); // 2
    }
}
```
Because the `hashCode()` method is not overridden in the Student class, the hash code of each object return the default hash code value. As a result, the `HashSet` considers the two objects as different, even though they are equal according to the `equals()` method.

> [!NOTE]  
> By default, the `equals()` method compares two objects based on their memory addresses. This is why you should override the `equals()` method in your classes to perform semantic comparisons.    
> By default, the 'hashCode()' method returns a unique hash code for each object based the memory address of the object. This is why you should override the 'hashCode()' method in your classes to return the same hash code for equal objects.