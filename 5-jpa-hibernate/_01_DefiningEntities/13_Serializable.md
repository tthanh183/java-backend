### Serializable Interface

1. **Introduction**

In this tutorial, we’ll discuss how JPA entities and the Java Serializable interface blend. First, we’ll take a look at the java.io.Serializable interface and why we need it. After that, we’ll take a look at the JPA specification and Hibernate as its most popular implementation.

2. **What is Serializable Interface?**

Serializable is one of the few marker interfaces found in core Java. Marker interfaces are special case interfaces with no methods or constants.

Object serialization is the process of converting Java objects into byte streams. We can then transfer these byte streams over the wire or store them in persistent memory. Deserialization is the reverse process, where we take byte streams and convert them back into Java objects. To allow object serialization (or deserialization), a class must implement the Serializable interface. Otherwise, we’ll run into java.io.NotSerializableException. Serialization is widely used in technologies such as RMI, JPA, and EJB.

3. **JPA and Serializable**

Let’s see what the JPA specification says about Serializable and how it pertains to Hibernate.

- JPA Specification:

One of the core parts of JPA is an entity class. We mark such classes as entities (either with the @Entity annotation or an XML descriptor). There are several requirements that our entity class must fulfill, and the one we’re most concerned with, according to the JPA specification, is:

> [!TIP]  
> If an entity instance is to be passed by value as a detached object (e.g., through a remote interface), the entity class must implement the Serializable interface.

In practice, if our object is to leave the domain of the JVM, it’ll require serialization.

Each entity class consists of persistent fields and properties. The specification requires that fields of an entity may be Java primitives, Java serializable types, or user-defined serializable types.

An entity class must also have a primary key. Primary keys can be primitive (single persistent field) or composite. Multiple rules apply to a composite key, one of which is that a composite key is required to be serializable.

Let’s create a simple example using Hibernate, H2 in-memory database, and a User domain object with UserId as a composite key:

```java
@Entity
public class User {
    @EmbeddedId UserId userId;
    String email;
    
    // constructors, getters and setters
}

@Embeddable
public class UserId implements Serializable{
    private String name;
    private String lastName;
    
    // getters and setters
}
```

We can test our domain definition using the integration test:

```java
@Test
public void givenUser_whenPersisted_thenOperationSuccessful() {
    UserId userId = new UserId();
    userId.setName("John");
    userId.setLastName("Doe");
    User user = new User(userId, "johndoe@gmail.com");

    entityManager.persist(user);

    User userDb = entityManager.find(User.class, userId);
    assertEquals(userDb.email, "johndoe@gmail.com");
}
```

If our UserId class does not implement the Serializable interface, we’ll get a MappingException with a concrete message that our composite key must implement the interface.

- Hibernate @JoinColumn Annotation:

Hibernate official documentation, when describing mapping in Hibernate, notes that the referenced field must be serializable when we use referencedColumnName from the @JoinColumn annotation. Usually, this field is a primary key in another entity. In rare cases of complex entity classes, our reference must be serializable.

Let’s extend the previous User class where the email field is no longer a String but an independent entity. Also, we’ll add an Account class that will reference a user and has a field type. Each User can have multiple accounts of different types. We’ll map Account by email since it’s more natural to search by email address:

```java
@Entity
public class User {
    @EmbeddedId private UserId userId;
    private Email email;
}

@Entity
public class Email implements Serializable {
    @Id
    private long id;
    private String name;
    private String domain;
}

@Entity
public class Account {
    @Id
    private long id;
    private String type;
    @ManyToOne
    @JoinColumn(referencedColumnName = "email")
    private User user;
}
```

To test our model, we’ll write a test where we create two accounts for a user and query by an email object:

```java
@Test
public void givenAssociation_whenPersisted_thenMultipleAccountsWillBeFoundByEmail() {
    // object creation 

    entityManager.persist(user);
    entityManager.persist(account);
    entityManager.persist(account2);

    List<Account> userAccounts = entityManager.createQuery("select a from Account a join fetch a.user where a.user.email = :email", Account.class)
      .setParameter("email", email)
      .getResultList();
    
    assertEquals(userAccounts.size(), 2);
}
```

NOTE: user is a reserved word in the H2 database and cannot be used to the name of an entity.

If the Email class does not implement the Serializable interface, we’ll get MappingException again, but this time with a somewhat cryptic message: “Could not determine type”.

- Exposing Entities to the Presentation Layer:

When sending objects over the wire using HTTP, we usually create specific DTOs (data transfer objects) for this purpose. By creating DTOs, we decouple internal domain objects from external services. If we want to expose our entities directly to the presentation layer without DTOs, then entities must be serializable.

We use the HttpSession object to store relevant data that help us identify users across multiple page visits to our website. The web server can store session data on a disk when shutting down gracefully or transfer session data to another web server in clustered environments. If an entity is part of this process, then it must be serializable. Otherwise, we’ll run into NotSerializableException.

> [!TIP]
> Serialization is required in the following cases: 
> - When an entity is detached and passed outside the JVM (e.g., via remote calls or REST API)
> - When an entity contains complex field types or collections that need to be transferred across JVMs or systems.
> - When a composite key (e.g., @EmbeddedId or @IdClass) is used, ensuring proper entity mapping and persistence