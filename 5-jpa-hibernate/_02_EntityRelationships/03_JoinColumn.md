### Join Column

1. **Overview**:

The annotation jakarta.persistence.JoinColumn marks a column as a join column for an entity association or an element collection.

In this quick tutorial, we’ll show some examples of basic @JoinColumn usage.

2. **@OneToOne Mapping Example**:

The @JoinColumn annotation combined with a @OneToOne mapping indicates that a given column in the owner entity refers to a primary key in the reference entity:

```java
@Entity
public class Office {
    @OneToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "addressId")
    private Address address;
}
```

The above code example will create a foreign key linking the Office entity with the primary key from the Address entity. The name of the foreign key column in the Office entity is specified by name property.

3. **@ManyToOne Mapping Example**:

When using a @OneToMany mapping, we can use the mappedBy parameter to indicate that the given column is owned by another entity:

```java
@Entity
public class Employee {
 
    @Id
    private Long id;

    @OneToMany(fetch = FetchType.LAZY, mappedBy = "employee")
    private List<Email> emails;
}

@Entity
public class Email {
 
    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "employee_id")
    private Employee employee;
}
```
In the above example, Email (the owner entity) has a join column employee_id that stores the id value and has a foreign key to the Employee entity.

![JoinColumn](https://www.baeldung.com/wp-content/uploads/2018/08/joincol1.png)

4. **@JoinColumns**:

In situations when we want to create multiple join columns, we can use the @JoinColumns annotation:

```java
@Entity
public class Office {
    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumns({
        @JoinColumn(name="ADDR_ID", referencedColumnName="ID"),
        @JoinColumn(name="ADDR_ZIP", referencedColumnName="ZIP")
    })
    private Address address;
}

```

The above example will create two foreign keys pointing to ID and ZIP columns in the Address entity:

![JoinColumns](https://www.baeldung.com/wp-content/uploads/2018/08/joincol2.png)