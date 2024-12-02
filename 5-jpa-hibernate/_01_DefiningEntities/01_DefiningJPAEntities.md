### Defining JPA Entities

Entities in JPA are nothing but POJOs representing data that can be persisted in the database. An entity represents a table stored in a database. Every instance of an entity represents a row in the table.

1. **The Entity Annotation**:

Let’s say we have a POJO called Student, which represents the data of a student, and we would like to store it in the database:

```java
public class Student {
    
    // fields, getters and setters
    
}
```

To do this, we should define an entity so that JPA is aware of it.

So let’s define it by making use of the @Entity annotation. We must specify this annotation at the class level. We must also ensure that the entity has a no-arg constructor and a primary key: 

```java
@Entity
public class Student {
    
    // fields, getters and setters
    
}
```
The entity name defaults to the name of the class. We can change its name using the name element:

```java
@Entity(name="student")
public class Student {
    
    // fields, getters and setters
    
}
```
Because various JPA implementations will try subclassing our entity to provide their functionality, entity classes must not be declared final.

2. **The Id Annotation**:

Each JPA entity must have a primary key that uniquely identifies it. The @Id annotation defines the primary key. We can generate the identifiers in different ways, which are specified by the @GeneratedValue annotation.

We can choose from four id generation strategies with the strategy element. The value can be AUTO, TABLE, SEQUENCE, or IDENTITY:

```java
@Entity
public class Student {
    @Id
    @GeneratedValue(strategy=GenerationType.AUTO)
    private Long id;
    
    private String name;
    
    // getters and setters
}
```

| GenerationType | Description                                                                                   |
|----------------|-----------------------------------------------------------------------------------------------|
| AUTO           | The persistence provider will pick the most appropriate strategy for the underlying database. |
| TABLE          | The persistence provider will use a database table to simulate a sequence.                    |
| SEQUENCE       | The persistence provider will use a database sequence to generate the primary key.            |
| IDENTITY       | The persistence provider will use a database identity column to generate the primary key.     |

If we annotate the entity’s fields, the JPA provider will use these fields to get and set the entity’s state. In addition to Field Access, we can also do Property Access or Mixed Access, which enables us to use both Field and Property access in the same entity.

> [!NOTE]  
> - Field Access: When you want the provider to bypass any custom logic in the getter/setter.
> - Property Access: When you have custom logic in your getter or setter that you want the provider to respect.
> - Mixed Access: When different attributes of the same entity require different access strategies.

3. **The Table Annotation**:

In most cases, the name of the table in the database and the name of the entity won’t be the same.

In these cases, we can specify the table name using the @Table annotation:

```java
@Entity
@Table(name="STUDENT")
public class Student {
    
    // fields, getters and setters
    
}
```

We can also mention the schema using the schema element:

```java
@Entity
@Table(name="STUDENT", schema="SCHOOL")
public class Student {
    
    // fields, getters and setters
    
}
```

If we don’t use the @Table annotation, the name of the table will be the name of the entity.

4. **The Column Annotation**:

Just like the @Table annotation, we can use the @Column annotation to mention the details of a column in the table.

The @Column annotation has many elements such as name, length, nullable, and unique:

```java
@Entity
@Table(name="STUDENT")
public class Student {
    @Id
    @GeneratedValue(strategy=GenerationType.AUTO)
    private Long id;
    
    @Column(name="STUDENT_NAME", length=50, nullable=false, unique=false)
    private String name;
    
    // other fields, getters and setters
}
```
The name element specifies the name of the column in the table. The length element specifies its length. The nullable element specifies whether the column is nullable or not, and the unique element specifies whether the column is unique.

If we don’t specify this annotation, the name of the column in the table will be the name of the field.

5. **The Transient Annotation**:

Sometimes, we may want to make a field non-persistent. We can use the @Transient annotation to do so. It specifies that the field won’t be persisted.

For instance, we can calculate the age of a student from the date of birth.

So let’s annotate the field age with the @Transient annotation:

```java
@Entity
@Table(name="STUDENT")
public class Student {
    @Id
    @GeneratedValue(strategy=GenerationType.AUTO)
    private Long id;
    
    @Column(name="STUDENT_NAME", length=50, nullable=false)
    private String name;
    
    @Transient
    private Integer age;
    
    // other fields, getters and setters
}
```

As a result, the field age won’t be persisted in the table.

6. **The Temporal Annotation**:

In some cases, we may have to save temporal values in our table.

```java
@Entity
@Table(name="STUDENT")
public class Student {
    @Id
    @GeneratedValue(strategy=GenerationType.AUTO)
    private Long id;
    
    @Column(name="STUDENT_NAME", length=50, nullable=false, unique=false)
    private String name;
    
    @Transient
    private Integer age;
    
    @Temporal(TemporalType.DATE)
    private Date birthDate;
    
    // other fields, getters and setters
}
```

However, with JPA 3.1, we also have support for java.time.LocalDate, java.time.LocalTime, java.time.LocalDateTime, java.time.OffsetTime and java.time.OffsetDateTime.

7. **The Enumerated Annotation**:

Sometimes, we may want to persist a Java enum type.

```java
public enum Gender {
    MALE, 
    FEMALE
}
```
```java
@Entity
@Table(name="STUDENT")
public class Student {
    @Id
    @GeneratedValue(strategy=GenerationType.AUTO)
    private Long id;
    
    @Column(name="STUDENT_NAME", length=50, nullable=false, unique=false)
    private String name;
    
    @Transient
    private Integer age;
    
    @Temporal(TemporalType.DATE)
    private Date birthDate;
    
    @Enumerated(EnumType.STRING)
    private Gender gender;
    
    // other fields, getters and setters
}
```

We don’t have to specify the @Enumerated annotation at all if we’re going to persist the Gender by the enum‘s ordinal.

However, to persist the Gender by enum name, we’ve configured the annotation with EnumType.STRING.