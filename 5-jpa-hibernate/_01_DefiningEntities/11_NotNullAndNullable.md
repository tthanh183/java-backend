### @NotNull vs @Column(nullable = false)

1. **Introduction**:

At first glance, it may seem like both the @NotNull and @Column(nullable = false) annotations serve the same purpose and can be used interchangeably. However, as we’ll soon see, this isn’t entirely true.

Even though, when used on the JPA entity, both of them essentially prevent storing null values in the underlying database, there are significant differences between these two approaches.

In this quick tutorial, we’ll compare the @NotNull and @Column(nullable = false) constraints.

2. **@NotNull**:

The @NotNull annotation is defined in the Bean Validation specification. This means its usage isn’t limited only to the entities. On the contrary, we can use @NotNull on any other bean as well.

Let’s stick with our use case though and add the @NotNull annotation to the Item‘s price field:

```java
@Entity
public class Item {

    @Id
    @GeneratedValue
    private Long id;

    @NotNull
    private BigDecimal price;
}
```

Now, let’s try to persist an item with a null price:

```java
@SpringBootTest
public class ItemIntegrationTest {

    @Autowired
    private ItemRepository itemRepository;

    @Test
    public void shouldNotAllowToPersistNullItemsPrice() {
        itemRepository.save(new Item());
    }
}
```

And let’s see Hibernate’s output:

```
org.springframework.dao.DataIntegrityViolationException: could not execute statement; SQL [n/a]; constraint [null]; nested exception is org.hibernate.exception.ConstraintViolationException: could not execute statement
```

As we can see, in this case, our system threw javax.validation.ConstraintViolationException.

It’s important to notice that Hibernate didn’t trigger the SQL insert statement. Consequently, invalid data wasn’t saved to the database.

This is because the pre-persist entity lifecycle event triggered the bean validation just before sending the query to the database.


- Schema Generation

In the previous section, we’ve presented how the @NotNull validation works.

Let’s now find out what happens if we let Hibernate generate the database schema for us.

For that reason, we’ll set a couple of properties in our application.properties file:

```properties
spring.jpa.hibernate.ddl-auto=create-drop
spring.jpa.show-sql=true
```

If we now start our application, we’ll see the DDL statement:

```sql  
create table item (
   id bigint not null,
    price decimal(19,2) not null,
    primary key (id)
)
```

Surprisingly, Hibernate automatically adds the not null constraint to the price column definition.

How’s that possible?

As it turns out, out of the box, Hibernate translates the bean validation annotations applied to the entities into the DDL schema metadata.

This is pretty convenient and makes a lot of sense. If we apply @NotNull to the entity, we most probably want to make the corresponding database column not null as well.

However, if, for any reason, we want to disable this Hibernate feature, all we need to do is to set hibernate.validator.apply_to_ddl property to false.

In order to test this let’s update our application.properties:

```properties
spring.jpa.hibernate.ddl-auto=create-drop
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.validator.apply_to_ddl=false
```

Let’s run the application and see the DDL statement:

```sql
create table item (
   id bigint not null,
    price decimal(19,2),
    primary key (id)
)
```

As expected, this time Hibernate didn’t add the not null constraint to the price column.

4. **@Column(nullable = false)**:

The @Column annotation is defined as a part of the Java Persistence API specification.

It’s used mainly in the DDL schema metadata generation. This means that if we let Hibernate generate the database schema automatically, it applies the not null constraint to the particular database column.

Let’s update our Item entity with the @Column(nullable = false) and see how this works in action:

```java
@Entity
public class Item {

    @Id
    @GeneratedValue
    private Long id;

    @Column(nullable = false)
    private BigDecimal price;
}
```

We can now try to persist a null price value:

```java
@SpringBootTest
public class ItemIntegrationTest {

    @Autowired
    private ItemRepository itemRepository;

    @Test
    public void shouldNotAllowToPersistNullItemsPrice() {
        itemRepository.save(new Item());
    }
}
```

Here’s the snippet of Hibernate’s output:

```
Hibernate: 
    
    create table item (
       id bigint not null,
        price decimal(19,2) not null,
        primary key (id)
    )

(...)

Hibernate: 
    insert 
    into
        item
        (price, id) 
    values
        (?, ?)
2019-11-14 13:23:03.000  WARN 14580 --- [main] o.h.engine.jdbc.spi.SqlExceptionHelper   : 
SQL Error: 23502, SQLState: 23502
2019-11-14 13:23:03.000 ERROR 14580 --- [main] o.h.engine.jdbc.spi.SqlExceptionHelper   : 
NULL not allowed for column "PRICE"
```

First of all, we can notice that Hibernate generated the price column with the not null constraint as we anticipated.

Additionally, it was able to create the SQL insert query and pass it through. As a result, it’s the underlying database that triggered the error.

- Validation

Almost all the sources emphasize that @Column(nullable = false) is used only for schema DDL generation.

Hibernate, however, is able to perform the validation of the entity against the possible null values, even if the corresponding field is annotated only with @Column(nullable = false).

In order to activate this Hibernate feature, we need to explicitly set the hibernate.check_nullability property to true:

```properties
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.check_nullability=true
```

Let’s now execute our test case again and examine the output:

```
org.springframework.dao.DataIntegrityViolationException: 
not-null property references a null or transient value : com.baeldung.h2db.springboot.models.Item.price; 
nested exception is org.hibernate.PropertyValueException: 
not-null property references a null or transient value : com.baeldung.h2db.springboot.models.Item.price
```

This time, our test case threw the org.hibernate.PropertyValueException.

It’s crucial to notice that, in this case, Hibernate didn’t send the insert SQL query to the database.

