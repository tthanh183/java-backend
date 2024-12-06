### One to One Relationship

1. **Overview**:

In this tutorial, we’ll have a look at different ways of creating one-to-one mappings in JPA.

2. **Description**:

Let’s suppose we are building a user management system, and our boss asks us to store a mailing address for each user. A user will have one mailing address, and a mailing address will have only one user tied to it.

This is an example of a one-to-one relationship, in this case between user and address entities.

Let’s see how we can implement this in the next sections.

3. **Using a Foreign Key**:

- Modeling with a foreign key.

Let’s have a look at the following ER diagram, which represents a foreign key-based one-to-one mapping:

![One to One Relationship](https://www.baeldung.com/wp-content/uploads/2018/12/1-1_FK.png)

- Implementing With a Foreign Key in JPA:

First, let’s create the User class and annotate it appropriately:

```java 
@Entity
@Table(name = "users")
public class User {
    
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    @Column(name = "id")
    private Long id;
    //... 

    @OneToOne(cascade = CascadeType.ALL)
    @JoinColumn(name = "address_id", referencedColumnName = "id")
    private Address address;

    // ... getters and setters
}
```

Note that we place the @OneToOne annotation on the related entity field, Address.

Also, we need to place the @JoinColumn annotation to configure the name of the column in the users table that maps to the primary key in the address table. If we don’t provide a name, Hibernate will follow some rules to select a default one.

Finally, note in the next entity that we won’t use the @JoinColumn annotation there. This is because we only need it on the owning side of the foreign key relationship. Simply put, whoever owns the foreign key column gets the @JoinColumn annotation.

The Address entity turns out a bit simpler:

```java
@Entity
@Table(name = "address")
public class Address {

    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    @Column(name = "id")
    private Long id;
    //...

    @OneToOne(mappedBy = "address")
    private User user;

    //... getters and setters
}
```

We also need to place the @OneToOne annotation here too. That’s because this is a bidirectional relationship. The address side of the relationship is called the non-owning side. 

4. **Using a Shared Primary Key**:

- Modeling with a shared primary key.

In this strategy, instead of creating a new column address_id, we’ll mark the primary key column (user_id) of the address table as the foreign key to the users table:

![One to One Relationship](https://www.baeldung.com/wp-content/uploads/2018/12/1-1-SK.png)

We’ve optimized the storage space by utilizing the fact that these entities have a one-to-one relationship between them.

- Implementing With a Shared Primary Key in JPA:

Notice that our definitions change only slightly:

```java
@Entity
@Table(name = "users")
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    @Column(name = "id")
    private Long id;

    //...

    @OneToOne(mappedBy = "user", cascade = CascadeType.ALL)
    @PrimaryKeyJoinColumn
    private Address address;

    //... getters and setters
}
```

```java
@Entity
@Table(name = "address")
public class Address {

    @Id
    @Column(name = "user_id")
    private Long id;

    //...

    @OneToOne
    @MapsId
    @JoinColumn(name = "user_id")
    private User user;
   
    //... getters and setters
}
```

The mappedBy attribute is now moved to the User class since the foreign key is now present in the address table. We’ve also added the @PrimaryKeyJoinColumn annotation, which indicates that the primary key of the User entity is used as the foreign key value for the associated Address entity.

We still have to define an @Id field in the Address class. But note that this references the user_id column, and it no longer uses the @GeneratedValue annotation. Also, on the field that references the User, we’ve added the @MapsId annotation, which indicates that the primary key values will be copied from the User entity.

5. **Using a Join Table**:

One-to-one mappings can be of two types: optional and mandatory. So far, we’ve seen only mandatory relationships.

Now let’s imagine that our employees get associated with a workstation. It’s one-to-one, but sometimes an employee might not have a workstation and vice versa.

- Modeling with a join table.

The strategies that we have discussed until now force us to put null values in the column to handle optional relationships.

Typically, we think of many-to-many relationships when we consider a join table, but using a join table in this case can help us to eliminate these null values:

![One to One Relationship](https://www.baeldung.com/wp-content/uploads/2018/12/1-1-JT.png)

Now, whenever we have a relationship, we’ll make an entry in the emp_workstation table and avoid nulls altogether.

- Implementing With a Join Table in JPA:

Our first example used @JoinColumn. This time, we’ll use @JoinTable:

```java
@Entity
@Table(name = "employee")
public class Employee {
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    @Column(name = "id")
    private Long id;

    //...

    @OneToOne(cascade = CascadeType.ALL)
    @JoinTable(name = "emp_workstation", 
      joinColumns = 
        { @JoinColumn(name = "employee_id", referencedColumnName = "id") },
      inverseJoinColumns = 
        { @JoinColumn(name = "workstation_id", referencedColumnName = "id") })
    private WorkStation workStation;

    //... getters and setters
}
```

```java
@Entity
@Table(name = "workstation")
public class WorkStation {

    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    @Column(name = "id")
    private Long id;

    //...

    @OneToOne(mappedBy = "workStation")
    private Employee employee;

    //... getters and setters
}
```

@JoinTable instructs Hibernate to employ the join table strategy while maintaining the relationship.

Also, Employee is the owner of this relationship, as we chose to use the join table annotation on it.