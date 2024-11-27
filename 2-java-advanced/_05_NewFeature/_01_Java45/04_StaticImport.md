### Static Import

The static import feature in Java was introduced in Java 5. It allows Java developers to directly access the static members of a class being imported, without the need to refer to the class name.

Advantages of static import: It helps reduce code when you need to access methods from other classes directly.  
Disadvantages of static import: If overused, static imports can make the code difficult to read.

Example:
```java
import static java.lang.Math.PI;
import static java.lang.Math.sqrt;

public class StaticImportExample {
    public static void main(String[] args) {
        double radius = 5;
        double area = PI * sqrt(radius);
        System.out.println("Area of the circle: " + area);
    }
}
```
Output:
```
Area of the circle: 15.707963267948966
```

Difference between static import and normal import:

| Static Import                                                                     | Normal Import                                                 |
|-----------------------------------------------------------------------------------|---------------------------------------------------------------|
| access to static members of a class without the need to reference the class name. | access classes from a package without using the package name. |
