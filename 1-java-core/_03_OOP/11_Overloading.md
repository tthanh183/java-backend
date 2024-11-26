### Method Overloading in Java

Method overloading in Java occurs when a class has multiple methods with the same name but different parameter types or number of parameters.
> [!NOTE]
> Method overloading is also known as compile-time polymorphism. It is achieved by the compiler based on the method signature.

There are two ways to overload a method in Java:
- Change the number of parameters
- Change the data type of the parameters
> [!NOTE]
> In Java, you cannot overload a method by changing only the return type.

1. **Method Overloading by Changing the Number of Parameters**
Example: 
```java
class Adder {
    static int add(int a, int b) {
        return a + b;
    }

    static int add(int a, int b, int c) {
        return a + b + c;
    }
}

class TestOverloading1 {
    public static void main(String[] args) {
        System.out.println(Adder.add(11, 11));
        System.out.println(Adder.add(11, 11, 11));
    }
}
```
Output:
```
22
33
```
2. **Method Overloading by Changing the Data Type of the Parameters**
Example:
```java
class Adder {
    static int add(int a, int b) {
        return a + b;
    }

    static double add(double a, double b) {
        return a + b;
    }
}

class TestOverloading2 {
    public static void main(String[] args) {
        System.out.println(Adder.add(11, 11));
        System.out.println(Adder.add(12.3, 12.6));
    }
}
```
Output:
```
22
24.9
```

### Frequently Asked Questions
Question: Why cant we overload a method by changing only the return type?  
Answer: In Java, method overloading is based on the method signature, which includes the method name and the parameter list. The return type is not part of the method signature, so changing only the return type does not create a new method signature. Therefore, you cannot overload a method by changing only the return type.  
Question: Can we overload the main method in Java?  
Answer: Yes, you can overload the main method in Java. However, the JVM will always look for the main method with the signature `public static void main(String[] args)`. If you overload the main method with a different signature, it will not be executed when you run the program.
