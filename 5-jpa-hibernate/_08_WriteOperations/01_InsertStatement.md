### Insert Statement

1. **Overview**

In this quick tutorial, we’ll learn how to perform an INSERT statement on JPA objects.

For more information about Hibernate in general, check out our comprehensive guide to JPA with Spring and introduction to Spring Data with JPA for deep dives into this topic.

2. **Persisting Objects in JPA**

In JPA, every entity going from a transient to managed state is automatically handled by the EntityManager.

The EntityManager checks whether a given entity already exists and then decides if it should be inserted or updated. Because of this automatic management, the only statements allowed by JPA are SELECT, UPDATE and DELETE.

In the examples below, we’ll look at different ways of managing and bypassing this limitation.

3. **Defining a Common Model**

Now, let’s start by defining a simple entity that we’ll use throughout this tutorial:

```java
@Entity
public class Person {

    @Id
    private Long id;
    private String firstName;
    private String lastName;

    // standard getters and setters, default and all-args constructors
}
```
Also, let’s define a repository class that we’ll use for our implementations:

```java
@Repository
public class PersonInsertRepository {

    @PersistenceContext
    private EntityManager entityManager;

}
```

Additionally, we’ll apply the @Transactional annotation to handle transactions automatically by Spring. This way, we won’t have to worry about creating transactions with our EntityManager, committing our changes, or performing rollback manually in the case of an exception.

4. **createNativeQuery**

For manually created queries, we can use the EntityManager#createNativeQuery method. It allows us to create any type of SQL query, not only ones supported by JPA. Let’s add a new method to our repository class:

```java
@Transactional
public void insertWithQuery(Person person) {
    entityManager.createNativeQuery("INSERT INTO person (id, first_name, last_name) VALUES (?,?,?)")
      .setParameter(1, person.getId())
      .setParameter(2, person.getFirstName())
      .setParameter(3, person.getLastName())
      .executeUpdate();
}
```

With this approach, we need to define a literal query including names of the columns and set their corresponding values.

We can now test our repository:

```java
@Test
public void givenPersonEntity_whenInsertedTwiceWithNativeQuery_thenPersistenceExceptionExceptionIsThrown() {
    Person person = new Person(1L, "firstname", "lastname");

    assertThatExceptionOfType(PersistenceException.class).isThrownBy(() -> {
        personInsertRepository.insertWithQuery(PERSON);
        personInsertRepository.insertWithQuery(PERSON);
    });
}
```

In our test, every operation attempts to insert a new entry into our database. Since we tried to insert two entities with the same id, the second insert operation fails by throwing a PersistenceException.

The principle here is the same if we are using Spring Data’s @Query.

5. **persist**

In our previous example, we created insert queries, but we had to create literal queries for each entity. This approach is not very efficient and results in a lot of boilerplate code.

Instead, we can make use of the persist method from EntityManager.

As in our previous example, let’s extend our repository class with a custom method:

```java
@Transactional
public void insertWithEntityManager(Person person) {
    this.entityManager.persist(person);
}
```

Now, we can test our approach again:

```java
@Test
public void givenPersonEntity_whenInsertedTwiceWithEntityManager_thenEntityExistsExceptionIsThrown() {
    assertThatExceptionOfType(EntityExistsException.class).isThrownBy(() -> {
        personInsertRepository.insertWithEntityManager(new Person(1L, "firstname", "lastname"));
        personInsertRepository.insertWithEntityManager(new Person(1L, "firstname", "lastname"));
    });
}
```

In contrast to using native queries, we don’t have to specify column names and corresponding values. Instead, EntityManager handles that for us.

In the above test, we also expect EntityExistsException to be thrown instead of its superclass PersistenceException which is more specialized and thrown by persist.

On the other hand, in this example, we have to make sure that we call our insert method each time with a new instance of Person. Otherwise, it will be already managed by EntityManager, resulting in an update operation.
