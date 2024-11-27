### Custom Annotation

Custom annotations in Java, or user-defined annotations, are easy to create and use. The @interface element is used to declare an annotation. For example:

```
@interface MyAnnotation {}
```

Three types of annotations can be created in Java:
- Marker annotation: An annotation without any elements.
- Single-value annotation: An annotation with a single element.
- Multi-value annotation: An annotation with multiple elements.

1. **Marker Annotation**:

```java
@interface MyAnnotation {}
```

`@Override` and `@Deprecated` are examples of marker annotations.
2. **Single-Value Annotation**:

An annotation with one method is called a single-value annotation. 

```java
@interface MyAnnotation {
    int value();
}
```

You can also provide a default value for the method:

```java
@interface MyAnnotation {
    int value() default 0;
}
```

How to apply a single-value annotation:

```java
@MyAnnotation(value = 10)
public class MyClass {}
```

3. **Multi-Value Annotation**:

An annotation with more than one method is called a multi-value annotation. Example:

```java
@interface MyAnnotation {
    int value1();
    String value2();
}
```

You can also provide default values:

```java
@interface MyAnnotation {
    int value1() default 0;
    String value2() default "default";
}
```

How to apply a multi-value annotation:

```java
@MyAnnotation(value1 = 10, value2 = "Hello")
public class MyClass {}
```

4. **Use built-in annotations in Custom Annotation**:

- `@Target`

The `@Target` annotation is used to specify where the custom annotation can be applied.

The `enum` `java.lang.annotation.ElementType` declares several constants to determine the type of element the annotation can be applied to, such as `TYPE`, `METHOD`, `FIELD`, etc. Here are the constants for ElementType:

| Constant          | Description                           |
|-------------------|---------------------------------------|
| `TYPE`            | Class, interface, or enum declaration |
| `FIELD`           | Field declaration                     |
| `METHOD`          | Method declaration                    |
| `CONSTURCTOR`     | Constructor declaration               |
| `LOCAL_VARIABLE`  | Local variable declaration            |
| `ANNOTATION_TYPE` | Annotation type declaration           |
| `PARAMETER`       | Parameter declaration                 |

Example:

```java
import java.lang.annotation.ElementType;
import java.lang.annotation.Target;

@Target(ElementType.METHOD)
@interface MyAnnotation {
    int value();
}
```

- `@Retention`

The @Retention annotation is used to specify at which level the annotation is available.   

`RetentionPolicy.SOURCE`: The annotation is only present in the source code and will be discarded during compilation. It won't be available in the compiled .class file.  
`RetentionPolicy.CLASS`: The annotation is available in the .class file but not at runtime.  
`RetentionPolicy.RUNTIME`: The annotation is available at runtime for reflection and is included in the .class file.  

- `@Inherited`

By default, annotations are not inherited by subclasses. The `@Inherited` annotation marks that an annotation is inherited by subclasses.

- `@Documented`

The `@Documented` annotation indicates that the annotation should be included in the Javadoc documentation.






