### Encapsulation
Encapsulation in Java is a technique used to hide unrelated information and display relevant information. 
The main purpose of encapsulation: 
- protect the internal state of an object by hiding the variables that represent the state of the object. Modifying an object is done and validated through methods.
- reduce the complexity of software development.

Example:
```java
public class Student {
    private int id;
    private String name;

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public static void main(String args[]) {
        Student s1 = new Student();
        s1.setId(111);
        s1.setName("Tran");

        System.out.println(s1.getId() + " " + s1.getName());
    }
}
```
Output:
``` 
111 Tran
```