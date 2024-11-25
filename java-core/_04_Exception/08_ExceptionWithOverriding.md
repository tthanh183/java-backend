### Exception Handling with Method Overriding in Java

1. **The parent class method does not declare an exception**
> [!NOTE]  
> If the parent class method does not declare an exception, the child class overridden method cannot declare the checked exception but can declare unchecked exceptions.

Example1: the parent class method does not declare an exception, and the child class method declares a checked exception
```java
import java.io.*;

class Parent {
    void msg() {
        System.out.println("parent");
    }
}

class TestExceptionChild extends Parent {
    void msg() throws IOException {
        System.out.println("child");
    }

    public static void main(String[] args) {
        Parent p = new TestExceptionChild();
        p.msg();
    }
}
```
Output:  
```
TestExceptionChild.java:9: error: unreported exception IOException; must be caught or declared to be thrown
    void msg() throws IOException {
                     ^
```
Example2: the parent class method does not declare an exception, and the child class method declares an unchecked exception
```java
import java.io.*;

class Parent {
    void msg() {
        System.out.println("parent");
    }
}

class TestExceptionChild extends Parent {
    void msg() throws ArithmeticException {
        System.out.println("child");
    }

    public static void main(String[] args) {
        Parent p = new TestExceptionChild();
        p.msg();
    }
}
```
Output:  
```
child
```

2**The parent class method not declares an exception**
> [!NOTE]
> If the parent class method declares an exception, the child class overridden method can declare the same, subclass exception, or no exception but cannot declare the parent exception.

Example1: the parent class method declares an exception, and the child class method declares the same exception
```java
import java.io.*;
    
class Parent {
    void msg() throws IOException {
        System.out.println("parent");
    }
}

class TestExceptionChild extends Parent {
    void msg() throws IOException {
        System.out.println("child");
    }

    public static void main(String[] args) {
        Parent p = new TestExceptionChild();
        try {
            p.msg();
        } catch (IOException e) {
            System.out.println(e);
        }
    }
}
```
Output:  
```
child
```

Example2: the parent class method declares an exception, and the child class method declares a subclass exception
```java
import java.io.*;

class Parent {
    void msg() throws IOException {
        System.out.println("parent");
    }
}

class TestExceptionChild extends Parent {
    void msg() throws FileNotFoundException {
        System.out.println("child");
    }

    public static void main(String[] args) {
        Parent p = new TestExceptionChild();
        try {
            p.msg();
        } catch (IOException e) {
            System.out.println(e);
        }
    }
}
```
Output:  
```
child
```

Example3: the parent class method declares an exception, and the child class method does
not declare an exception
```java
import java.io.*;

class Parent {
    void msg() throws IOException {
        System.out.println("parent");
    }
}

class TestExceptionChild extends Parent {
    void msg() {
        System.out.println("child");
    }

    public static void main(String[] args) {
        Parent p = new TestExceptionChild();
        try {
            p.msg();
        } catch (IOException e) {
            System.out.println(e);
        }
    }
}
```
Output:  
```
child
```

Example4: the parent class method declares an exception, and the child class method declares a parent exception
```java
import java.io.*;

class Parent {
    void msg() throws IOException {
        System.out.println("parent");
    }
}

class TestExceptionChild extends Parent {
    void msg() throws Exception {
        System.out.println("child");
    }

    public static void main(String[] args) {
        Parent p = new TestExceptionChild();
        try {
            p.msg();
        } catch (IOException e) {
            System.out.println(e);
        }
    }
}
```
Output:  
```
TestExceptionChild.java:9: error: msg() in TestExceptionChild cannot override msg() in Parent
    void msg() throws Exception {
         ^
  overridden method does not throw Exception
1 error
```
