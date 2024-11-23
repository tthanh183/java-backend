### Type Casting
Type casting in Java is the process of assigning the value of a variable of one data type to a variable of a different data type.
In Java, there are two types of type casting:
- Widening: This is the process of converting a value from a data type with a smaller size to a data type with a larger size. This type of casting does not lose any information.
- Narrowing: This is the process of converting a value from a data type with a larger size to a data type with a smaller size. This type of casting may result in a loss of information.

1. **Widening**
   ![](https://viettuts.vn/images/java/java-co-ban/ep-kieu-du-lieu-noi-rong.jpg)

Type casting (Widening): This is the process of converting a value from a data type with a smaller size to a data type with a larger size. This type of casting does not lose any information. For example, converting from int to float. This type of conversion can be performed implicitly by the compiler.

Example:
```java
package vn.com.example;

public class TestWidening {
    public static void main(String[] args) {
        int i = 100;
        long l = i;    // no need to specify type casting
        float f = l;   // no need to specify type casting
        System.out.println("Int value: " + i);
        System.out.println("Long value: " + l);
        System.out.println("Float value: " + f);
    }
}
```
Output:
```
Int value: 100
Long value: 100
Float value: 100.0
```
2. **Narrowing**
   ![](https://viettuts.vn/images/java/java-co-ban/ep-kieu-du-lieu-thu-hep.jpg)
   
Type casting (Narrowing): This is the process of converting a value from a data type with a larger size to a data type with a smaller size. This type of casting may result in a loss of information, as shown in the example above. This type of conversion cannot be performed implicitly by the compiler; the user must explicitly specify the type conversion.

Example:
```java
package vn.com.example;

public class TestNarrowing {
    public static void main(String[] args) {
        double d = 100.04;
        long l = (long) d; // requires explicit type casting to long
        int i = (int) l;   // requires explicit type casting to int
 
        System.out.println("Double value: " + d);
        System.out.println("Long value: " + l);
        System.out.println("Int value: " + i);
    }
}
```
Output:
```
Double value: 100.04
Long value: 100
Int value: 100
```