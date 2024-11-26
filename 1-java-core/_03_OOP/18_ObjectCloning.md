### Object Cloning in Java
Object cloning is a way to create an exact copy of an object that is being cloned. The clone() method is used to create a new object.

The class of the object that we want to clone must implement the java.lang.Cloneable interface. If we do not implement the Cloneable interface, calling the clone() method will result in a CloneNotSupportedException.

The clone() method is defined in the Object class with the following syntax:
```java
protected Object clone() throws CloneNotSupportedException;
```
### Why we use the clone() method?  
The clone() method saves processing tasks when creating an exact copy of an object. If we try to do it using the new keyword, a lot of processing tasks would be involved. This is why we use object cloning.

```java

class Student implements Cloneable {
    int rollno;
    String name;

    Student(int rollno, String name) {
        this.rollno = rollno;
        this.name = name;
    }

    public Object clone() throws CloneNotSupportedException {
        return super.clone();
    }

    public static void main(String[] args) {
        try {
            Student s1 = new Student(101, "John");
            Student s2 = (Student) s1.clone();

            System.out.println(s1.rollno + " " + s1.name);
            System.out.println(s2.rollno + " " + s2.name);
        } catch (CloneNotSupportedException c) {
            System.out.println(c);
        }
    }
}
```
Output:
```
101 John
101 John
```
