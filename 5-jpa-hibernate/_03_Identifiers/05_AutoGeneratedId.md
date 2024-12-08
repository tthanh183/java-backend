### Auto Generated Id

1. **Introduction**

In this tutorial, we’ll discuss how to handle auto-generated ids with JPA. There are two key concepts that we must understand before we take a look at a practical example, namely life cycle and id generation strategy.

2. **Entity Life Cycle and Id Generation**

Each entity has four possible states during its life cycle. Those states are new, managed, detached, and removed. Our focus will be on the new and managed states. During object creation, an entity is in the new state. Consequently, EntityManager is unaware of this object. Calling the persist method on EntityManager, the object transitions from a new to managed state. This method requires an active transaction.

JPA defines four strategies for id generation. We can group these four strategies into two categories:

- Ids are pre-allocated and available to EntityManager before commit
- Ids are allocated after transaction commit

For more details about each id generation strategy, refer to our article, When Does JPA Set the Primary Key.

3. **Problem Statement**

Returning an id of an object can become a cumbersome task. We need to understand the principles mentioned in the previous section to avoid issues. Depending on JPA configuration, services may return objects with id equal to zero (or null). The focus will be on service class implementation and how different modifications can provide us with a solution.

We’ll create a Maven module with the JPA specification and Hibernate as its implementation. For simplicity, we’ll use an H2 in-memory database.

Let’s start by creating a domain entity and mapping it to a database table. For this example, we’ll create a User entity with a few basic properties:

```java
@Entity
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private long id;
    private String username;
    private String password;
 
    //...
}
```

After the domain class, we’ll create a UserService class. This simple service will have a reference to EntityManager and a method to save User objects to the database:

```java
public class UserService {
    EntityManager entityManager;
 
    public UserService(EntityManager entityManager) {
        this.entityManager = entityManager;
    }
 
    @Transactional
    public long saveUser(User user){
        entityManager.persist(user);
        return user.getId();
    }
}
```

This setup is a common pitfall that we previously mentioned. We can prove that the return value of the saveUser method is zero with a test:

```java
@Test
public void whenNewUserIsPersisted_thenEntityHasNoId() {
    User user = new User();
    user.setUsername("test");
    user.setPassword(UUID.randomUUID().toString());
 
    long index = service.saveUser(user);
 
    Assert.assertEquals(0L, index);
}
```

In the following sections, we’ll step back to understand why this happened, and how we can solve it.

4. **Manual Transaction Control**

After object creation, our User entity is in the new state. The entity state changes to managed after the persist method call in the saveUser method. We remember from the recap section that the managed object gets an id after the transaction commit. Since the saveUser method is still running, the transaction created by the @Transactional annotation isn’t yet committed. Our managed entity gets an id when saveUser finishes execution.

One possible solution is to call the flush method on EntityManager manually. On the other hand, we can manually control transactions and guarantee that our method returns the id correctly. We can do this with EntityManager:

```java
@Test
public void whenTransactionIsControlled_thenEntityHasId() {
    User user = new User();
    user.setUsername("test");
    user.setPassword(UUID.randomUUID().toString());
     
    entityManager.getTransaction().begin();
    long index = service.saveUser(user);
    entityManager.getTransaction().commit();
     
    Assert.assertEquals(2L, index);
}
```

5. **Using Id Generation Strategies**

Up until now, we used the second category, where id allocation occurs after the transaction commit. Pre-allocating strategies can provide us with ids before the transaction commit, since they keep a handful of ids in memory. This option isn’t always possible to implement because not all database engines support all generation strategies. Changing the strategy to GenerationType.SEQUENCE can resolve our problem. This strategy uses a database sequence instead of an auto-incrementing column as in GenerationType.IDENTITY.

To change the strategy, we edit our domain entity class:

```java
@Entity
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.SEQUENCE)
    private long id;
 
    //...
}
```




