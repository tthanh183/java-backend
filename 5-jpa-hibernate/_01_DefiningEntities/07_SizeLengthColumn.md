### Difference Between @Size, @Length, and @Column(length=value)

1. **Overview**:

In this quick tutorial, we’ll take a look at JSR-330‘s @Size annotation, Hibernate‘s @Length annotation, and JPA‘s @Column‘s length attribute.

At first blush, these may seem the same, but they perform different functions.

2. **Origins**:

Simply put, all of these annotations are meant to communicate the size of a field.

@Size and @Length are similar. We can use either annotation to validate the size of a field. The former is a Java-standard annotation, while the latter is specific to Hibernate.

@Column, though, is a JPA annotation that we use to control DDL statements.

Now let’s go through each of them in detail.

3. **@Size**:

For validations, we’ll use @Size, a bean validation annotation. We’ll use the property middleName annotated with @Size to validate its value between the attributes min and max:

```java
public class User {

    // ...

    @Size(min = 3, max = 15)
    private String middleName;

    // ...

}
```
Most importantly, @Size makes the bean independent of JPA and its vendors, such as Hibernate. As a result, it’s more portable than @Length.

4. **@Length**:

As we previously mentioned, @Length is the Hibernate-specific version of @Size. We’ll enforce the range for lastName using @Length:

```java
@Entity
public class User {

    // ...
      
    @Length(min = 3, max = 15)
    private String lastName;

    // ...

}
```

5. **@Column(length=value)**:

@Column is quite different from the previous two annotations.  
We’ll use @Column to indicate specific characteristics of the physical database column. We’ll use the length attribute of the @Column annotation to specify the string-valued column length:

```java
@Entity
public class User {

    @Column(length = 3)
    private String firstName;

    // ...

}   
```

The resulting column will be generated as a VARCHAR(3), and trying to insert a longer string will result in an SQL error.

Note that we’ll use @Column only to specify table column properties, as it doesn’t provide validations.

Of course, we can use @Column together with @Size to specify database column properties with bean validation:
    
```java
@Entity
public class User {

    // ... 
    
    @Column(length = 5)
    @Size(min = 3, max = 5)
    private String city;

    // ...

}
```