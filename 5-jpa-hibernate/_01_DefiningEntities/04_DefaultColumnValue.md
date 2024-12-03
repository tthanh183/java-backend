### Default Column Value

1. **Overview**:

In this tutorial, we’ll look into default column values in JPA.

We’ll learn how to set them as a default property in the entity as well as directly in the SQL table definition.

2. **While Creating an Entity**:

The first way to set a default column value is to set it directly as an entity property value:  

```java
@Entity
public class User {
    @Id
    private Long id;
    private String firstName = "John Snow";
    private Integer age = 25;
    private Boolean locked = false;
}
```

Now, every time we create an entity using the new operator, it will set the default values we’ve provided:

```java
@Test
void givenUser_whenUsingEntityValue_thenSavedWithDefaultValues() {
    User user = new User();
    user = userRepository.save(user);
    
    assertEquals(user.getName(), "John Snow");
    assertEquals(user.getAge(), 25);
    assertFalse(user.getLocked());
}
```

There is one drawback to this solution.

When we take a look at the SQL table definition, we won’t see any default value in it:

```sql
create table user
(
    id     bigint not null constraint user_pkey primary key,
    name   varchar(255),
    age    integer,
    locked boolean
);
```

So, if we override them with null, the entity will be saved without any error:

```java
@Test
void givenUser_whenNameIsNull_thenSavedNameWithNullValue() {
    User user = new User();
    user.setName(null);
    user.setAge(null);
    user.setLocked(null);
    user = userRepository.save(user);

    assertNull(user.getName());
    assertNull(user.getAge());
    assertNull(user.getLocked());
}
``` 

3. **In the Schema Definition**:

To create a default value directly in the SQL table definition, we can use the @Column annotation and set its columnDefinition parameter:

```java
@Entity
public class User {
    @Id
    Long id;

    @Column(columnDefinition = "varchar(255) default 'John Snow'")
    private String name;

    @Column(columnDefinition = "integer default 25")
    private Integer age;

    @Column(columnDefinition = "boolean default false")
    private Boolean locked;
}
```

Using this method, the default value will be present in the SQL table definition:

```sql
create table user
(
    id     bigint not null constraint user_pkey primary key,
    name   varchar(255) default 'John Snow',
    age    integer      default 35,
    locked boolean      default false
);
```

And the entity will be saved properly with the default values:

```java
@Test
void givenUser_whenUsingSQLDefault_thenSavedWithDefaultValues() {
    User user = new User();
    user = userRepository.save(user);

    assertEquals(user.getName(), "John Snow");
    assertEquals(user.getAge(), 25);
    assertFalse(user.getLocked());
}
```

Remember that by using this solution, we won’t be able to set a given column to null when saving the entity for the first time. The default one will be set automatically if we don’t provide any value.

4. **Using @ColumnDefault**:

The @ColumnDefault annotation offers another way to specify default values for columns. It’s used to set a default value at the JPA level, which is reflected in the generated SQL schema.

Here’s an example of how to use @ColumnDefault to define the default value:

```java
@Entity
@Table(name="users_entity")
public class UserEntity {
    @Id
    private Long id;

    @ColumnDefault("John Snow")
    private String name;

    @ColumnDefault("25")
    private Integer age;

    @ColumnDefault("false")
    private Boolean locked;
}
```

When using Hibernate as the JPA provider, the @ColumnDefault annotations will be reflected in the SQL schema generation, resulting in a table creation SQL like this:

```sql
create table users_entity 
(
    age integer default 25,
    locked boolean default false,
    id bigint not null,
    name varchar(255) default 'John Snow',
    primary key (id)
)
```

However, when inserting a row with only the id column, the default values for name, age, and locked will be applied automatically only if we omit them in the INSERT statement:

```java
INSERT INTO users_entity (id) VALUES (?);
```

If we include all columns in our INSERT statement, fields that aren’t set will be left as null, and the database won’t apply the default values.

Let’s specify the INSERT query by omitting other fields:

```java
void givenUser_whenSaveUsingQuery_thenSavedWithDefaultValues() {
    EntityManager entityManager = // initialize entity manager

    Query query = entityManager.createNativeQuery("INSERT INTO users_entity (id) VALUES(?) ");
    query.setParameter(1, 2L);
    query.executeUpdate();

    user = userEntityRepository.find(2L);
    assertEquals(user.getName(), "John Snow");
    assertEquals(25, (int) user.getAge());
    assertFalse(user.getLocked());
}
```

5. **Using @PrePersist**:

Additionally, we can handle default values by using the @PrePersist callback. This method allows us to set default values programmatically before the entity is persisted. This can be useful if we want to ensure that default values are set even if the entity isn’t properly initialized or if certain fields are null.

Here is an example of using @PrePersist:
```java
@Entity
public class UserEntity {
    @Id
    private Long id;

    private String name;
    private Integer age;
    private Boolean locked;

    @PrePersist
    public void prePersist() {
        if (name == null) {
            name = "John Snow";
        }
        if (age == null) {
            age = 25;
        }
        if (locked == null) {
            locked = false;
        }
    }
}
```

The @PrePersist method sets default values before saving the entity to the database. This ensures that all default values are applied if they weren’t provided.

By leveraging @PrePersist, our entity always has the default values applied, regardless of whether the fields were explicitly set to null:

```java
The @PrePersist method sets default values before saving the entity to the database. This ensures that all default values are applied if they weren’t provided.

By leveraging @PrePersist, our entity always has the default values applied, regardless of whether the fields were explicitly set to null:
```
