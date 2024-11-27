### Type Inference

In Java 7, Java provides an improved compiler that is smart enough to infer the creation of Generic objects. Now, you can replace the specific type arguments with an empty set of type parameters (<>). This pair of angle brackets is called the diamond operator.

The approach used in Java 6 and earlier versions was:
```
List<Integer> list = new List<Integer>();
```

In Java 7, you can use the diamond operator to simplify the code:
```
List<Integer> list = new List<>();
```

Type inference with Generic constructors is also possible in Java 7. The following example demonstrates how to use the diamond operator with a Generic constructor:
```java
class GenericClass<X> {                                
    // Generic constructor with type inference
    <T> GenericClass(T t) {
        System.out.println(t); 
    }
}

public class TypeInference2 {
    public static void main(String[] args) {
        GenericClass<String> gc2 = new GenericClass<>("Hello");
    }
}

```
