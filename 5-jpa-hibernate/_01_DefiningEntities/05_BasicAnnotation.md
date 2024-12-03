### Basic Annotation

1. **Overview**:

In this quick tutorial, we’ll explore the JPA @Basic annotation. We’ll also discuss the difference between @Basic and @Column JPA annotations.

2. **@Basic Types**:

JPA support various Java data types as persistable fields of an entity, often known as the basic types.

A basic type maps directly to a column in the database. These include Java primitives and their wrapper classes, String, java.math.BigInteger and java.math.BigDecimal, various available date-time classes, enums, and any other type that implements java.io.Serializable.

Hibernate, like any other ORM vendor, maintains a registry of basic types and uses it to resolve a column’s specific org.hibernate.type.Type.

3. **@Basic Annotation**:

We can use the @Basic annotation to mark a basic type property:

```
@Entity
public class Course {

    @Basic
    @Id
    private int id;

    @Basic
    private String name;
    ...
}
```
In other words, the @Basic annotation on a field or a property signifies that it’s a basic type and Hibernate should use the standard mapping for its persistence.

Note that it’s an optional annotation. And so, we can rewrite our Course entity as:
```
@Entity
public class Course {

    @Id
    private int id;

    private String name;
    ...
}
```
When we don’t specify the @Basic annotation for a basic type attribute, it is implicitly assumed, and the default values of this annotation apply.

4. **Why Use @Basic Annotation?**:

The @Basic annotation has two attributes, optional and fetch. Let’s take a closer look at each one.

The optional attribute is a boolean parameter that defines whether the marked field or property allows null. It defaults to true. So, if the field is not a primitive type, the underlying column is assumed to be nullable by default.

The fetch attribute accepts a member of the enumeration Fetch, which specifies whether the marked field or property should be lazily loaded or eagerly fetched. It defaults to FetchType.EAGER, but we can permit lazy loading by setting it to FetchType.LAZY.

Lazy loading will only make sense when we have a large Serializable object mapped as a basic type, as in that case, the field access cost can be significant.

We have a detailed tutorial covering Eager/Lazy loading in Hibernate that takes a deeper dive into the topic.

Now, let’s say don’t want to allow nulls for our Course‘s name and want to lazily load that property as well. Then, we’ll define our Course entity as:

```
@Entity
public class Course {
    
    @Id
    private int id;
    
    @Basic(optional = false, fetch = FetchType.LAZY)
    private String name;
    ...
}
```
We should explicitly use the @Basic annotation when willing to deviate from the default values of optional and fetch parameters. We can specify either one or both of these attributes, depending on our needs.

5. **@Column vs @Basic**:

Let’s look at the differences between @Basic and @Column annotations:
- Attributes of the @Basic annotation are applied to JPA entities, whereas the attributes of @Column are applied to the database columns
- @Basic annotation’s optional attribute defines whether the entity field can be null or not; on the other hand, @Column annotation’s nullable attribute specifies whether the corresponding database column can be null
- We can use @Basic to indicate that a field should be lazily loaded
- The @Column annotation allows us to specify the name of the mapped database column
