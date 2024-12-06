### @JoinColumn And mappedBy

1. **Overview**:

JPA Relationships can be either unidirectional or bidirectional. This simply means we can model them as an attribute on exactly one of the associated entities or both.

Defining the direction of the relationship between entities has no impact on the database mapping. It only defines the directions in which we use that relationship in our domain model.

For a bidirectional relationship, we usually define

the owning side
inverse or the referencing side
The @JoinColumn annotation helps us specify the column we’ll use for joining an entity association or element collection. On the other hand, the mappedBy attribute is used to define the referencing side (non-owning side) of the relationship.

In this quick tutorial, we’ll look at the difference between @JoinColumn and mappedBy in JPA. We’ll also present how to use them in a one-to-many association.

2**Initial Setup**:

To follow along with this tutorial, let’s say we have two entities: Employee and Email.

Clearly, an employee can have multiple email addresses. However, a given email address can belong exactly to a single employee.

This means they share a one-to-many association:

![JoinColumn](https://www.baeldung.com/wp-content/uploads/2018/11/12345789.png)

Also in our RDBMS model, we’ll have a foreign key employee_id in our Email entity referring to the id attribute of an Employee.

3. **@JoinColumn**:

In a One-to-Many/Many-to-One relationship, the owning side is usually defined on the many side of the relationship. It’s usually the side that owns the foreign key.

The @JoinColumn annotation defines that actual physical mapping on the owning side:

```java
@Entity
public class Email {

    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    private Long id;

    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "employee_id")
    private Employee employee;

    // ...

}
```

It simply means that our Email entity will have a foreign key column named employee_id referring to the primary attribute id of our Employee entity.

4. **mappedBy**:

Once we have defined the owning side of the relationship, Hibernate already has all the information it needs to map that relationship in our database.

To make this association bidirectional, all we’ll have to do is to define the referencing side. The inverse or the referencing side simply maps to the owning side.

We can easily use the mappedBy attribute of @OneToMany annotation to do so.

So, let’s define our Employee entity:

```java
@Entity
public class Employee {

    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    private Long id;

    @OneToMany(fetch = FetchType.LAZY, mappedBy = "employee")
    private List<Email> emails;
    
    // ...
}
```

Here, the value of mappedBy is the name of the association-mapping attribute on the owning side. With this, we have now established a bidirectional association between our Employee and Email entities.
