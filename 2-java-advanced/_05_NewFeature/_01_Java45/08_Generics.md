### Generics

Java Generics was introduced in Java 5. Generics in Java is a way to define specific types for classes and methods in different contexts. It might sound a bit abstract, so we will examine the concepts and provide some concrete examples.

1 **Generic Class**  
A class that can reference any type of object is called a generic class.

Example:
```java
public class Box<T> {
    private T t;

    public void set(T t) {
        this.t = t;
    }

    public T get() {
        return t;
    }

    public static void main(String[] args) {
        Box<Integer> integerBox = new Box<>();
        Box<String> stringBox = new Box<>();

        integerBox.set(10);
        stringBox.set("Hello World");

        System.out.println("Integer Value: " + integerBox.get());
        System.out.println("String Value: " + stringBox.get());
    }
}
```
Output:
``` 
Integer Value: 10
String Value: Hello World
```

In the class above, `Box<T>` is a generic class that can store any type of object. The type `T` is a placeholder for the actual type that will be used when creating an instance of the `Box` class. In the `main` method, we create two instances of the `Box` class, one for `Integer` and one for `String`.

> [!NOTE]  
> Naming conventions: 
> - E - Element (used extensively by the Java Collections Framework)
> - K - Key
> - N - Number
> - T - Type
> - V - Value

2. **Generic Method**

Like generic classes, we can create generic methods that can accept any type of arguments.

Example:
```java

public class GenericMethod {
    public static <T> void printArray(T[] array) {
        for (T element : array) {
            System.out.print(element + " ");
        }
        System.out.println();
    }

    public static void main(String[] args) {
        Integer[] intArray = {1, 2, 3, 4, 5};
        Double[] doubleArray = {1.1, 2.2, 3.3, 4.4, 5.5};
        Character[] charArray = {'H', 'E', 'L', 'L', 'O'};

        System.out.println("Integer Array:");
        printArray(intArray);

        System.out.println("Double Array:");
        printArray(doubleArray);

        System.out.println("Character Array:");
        printArray(charArray);
    }
}
```
Output:
```
Integer Array:
1 2 3 4 5
Double Array:
1.1 2.2 3.3 4.4 5.5
Character Array:
H E L L O
```

3. **Wildcard in Generics**

The wildcard `?` is used to represent an unknown type in generics. f we write <? extends Number>, it means any subclass of Number, such as Integer, Float, Double, etc. We can then call the methods of the Number class through any of its subclasses.

Example:
```java

public class WildcardExample {
    public static void printList(List<?> list) {
        for (Object element : list) {
            System.out.print(element + " ");
        }
        System.out.println();
    }

    public static void main(String[] args) {
        List<Integer> integerList = Arrays.asList(1, 2, 3);
        List<String> stringList = Arrays.asList("One", "Two", "Three");

        System.out.println("Integer List:");
        printList(integerList);

        System.out.println("String List:");
        printList(stringList);
    }
}
```
Output:
```
Integer List:
1 2 3
String List:
One Two Three
```

4. **Using Generics in Collections**

Before Generics, we could store any type of object in a collection like a non-generic collection. With Generics, we can specify the types of objects stored in collections.

Syntax:
```
ClassOrInterface<Type>
```

Example:
```java
List<String> list = new ArrayList<String>();
```

Example:
```java

public class GenericsInCollections {
    public static void main(String[] args) {
        List<String> list = new ArrayList<String>();
        list.add("Java");
        list.add("Python");
        list.add("C++");

        for (String language : list) {
            System.out.println(language);
        }
    }
}
```
Output:
```
Java
Python
C++
```

5. **The Advantages of Generics**

- Type Safety: Generics provide compile-time type checking, which helps prevent runtime errors.
- Code Reusability: Generics allow us to write reusable code that can be used with different types.
- Performance: Generics can improve performance by eliminating the need for typecasting.
- Eliminates the need for casting: Generics eliminate the need for explicit casting, making the code cleaner and more readable.
- Compile-time error checking: Generics provide compile-time error checking, which helps catch errors early in the development process.



