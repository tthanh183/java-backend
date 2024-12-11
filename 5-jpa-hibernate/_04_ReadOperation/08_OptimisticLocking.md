### Optimistic Locking

1. **Overview**

When it comes to enterprise applications, properly managing concurrent access to a database is crucial. This means we should be able to handle multiple transactions in an effective and, most importantly, error-proof way.

What’s more, we need to ensure that data stays consistent between concurrent reads and updates.

To achieve that, we can use an optimistic locking mechanism provided by Java Persistence API. This way, multiple updates made on the same data at the same time do not interfere with each other.

2. **Understanding Optimistic Locking** 

In order to use optimistic locking, we need to have an entity including a property with @Version annotation. While using it, each transaction that reads data holds the value of the version property.

Before the transaction wants to make an update, it checks the version property again.

If the value has changed in the meantime, an OptimisticLockException is thrown. Otherwise, the transaction commits the update and increments a value version property.

3. **Pessimistic Locking vs Optimistic Locking**

In contrast to optimistic locking, JPA gives us pessimistic locking. It’s another mechanism for handling concurrent access for data.

We cover pessimistic locking in one of our previous articles — Pessimistic Locking in JPA. Let’s find out the difference between them and how we can benefit from each type of locking.

As we’ve said before, optimistic locking is based on detecting changes on entities by checking their version attribute. If any concurrent update takes place, OptmisticLockException occurs. After that, we can retry updating the data.

We can imagine that this mechanism is suitable for applications that do many more reads than updates or deletes. It’s also useful in situations where entities must be detached for some time and locks cannot be held.

On the contrary, the pessimistic locking mechanism involves locking entities on the database level.

Each transaction can acquire a lock on data. As long as it holds the lock, no transaction can read, delete or make any updates on the locked data.

We can presume that using pessimistic locking may result in deadlocks. However, it ensures greater integrity of data than optimistic locking.

4. **Version Attributes**

Version attributes are properties with @Version annotation. They are necessary for enabling optimistic locking.

Let’s see a sample entity class:

```java
@Entity
public class Student {

    @Id
    private Long id;

    private String name;

    private String lastName;

    @Version
    private Integer version;

    // getters and setters

}
```

There are several rules that we should follow while declaring version attributes:

- Each entity class must have only one version attribute.
- It must be placed in the primary table for an entity mapped to several tables.
- Type of a version attribute must be one of the following: int, Integer, long, Long, short, Short, java.sql.Timestamp.

We should know that we can retrieve a value of the version attribute via entity, but we shouldn’t update or increment it. Only the persistence provider can do that, so data stays consistent.

Notice that persistence providers are able to support optimistic locking for entities that don’t have version attributes. Yet, it’s a good idea to always include version attributes when working with optimistic locking.

If we try to lock an entity that does not contain such attributes and the persistence provider does not support it, it will result in a PersistenceException.

5. **Lock Modes**

JPA provides us with two different optimistic lock modes (and two aliases):

- OPTIMISTIC obtains an optimistic read lock for all entities containing a version attribute.
- OPTIMISTIC_FORCE_INCREMENT obtains an optimistic lock the same as OPTIMISTIC and additionally increments the version attribute value.
- READ is a synonym for OPTIMISTIC.
- WRITE is a synonym for OPTIMISTIC_FORCE_INCREMENT.

We can find all types listed above in the LockModeType class.

- OPTIMISTIC (READ)

As we already know, OPTIMISTIC and READ lock modes are synonyms. However, JPA specification recommends us to use OPTIMISTIC in new applications.

Whenever we request the OPTIMISTIC lock mode, a persistence provider will prevent our data from dirty reads as well as non-repeatable reads.

Put simply, it should ensure any transaction fails to commit any modification on data that another transaction

- has updated or deleted but not committed
- has updated or deleted successfully in the meantime

- OPTIMISTIC_INCREMENT (WRITE)

The same as before, OPTIMISTIC_INCREMENT and WRITE are synonyms, but the former is preferable.

OPTIMISTIC_INCREMENT must meet the same conditions as OPTIMISTIC lock mode. Additionally, it increments the value of a version attribute. However, it’s not specified whether it should be done immediately or may be put off until commit or flush.

It’s worth knowing that a persistence provider is allowed to provide OPTIMISTIC_INCREMENT functionality when OPTIMISTIC lock mode is requested.

6. **Using Optimistic Locking**

We should remember that for versioned entities optimistic locking is available by default. But there are several ways of requesting it explicitly.

- Find

To request optimistic locking, we can pass the proper LockModeType as an argument to find method of EntityManager:

```
entityManager.find(Student.class, studentId, LockModeType.OPTIMISTIC);
```

- Query

Another way to enable locking is using the setLockMode method of Query object:

```
Query query = entityManager.createQuery("from Student where id = :id");
query.setParameter("id", studentId);
query.setLockMode(LockModeType.OPTIMISTIC_INCREMENT);
query.getResultList()
```

- Explicit Locking

We can set a lock by calling EntityManager’s lock method:

```
Student student = entityManager.find(Student.class, id);
entityManager.lock(student, LockModeType.OPTIMISTIC);
```

- Refresh

We can call the refresh method the same way as the previous method:

```
Student student = entityManager.find(Student.class, id);
entityManager.refresh(student, LockModeType.READ);
```

- NamedQuery

The last option is to use @NamedQuery with the lockMode property:

```
@NamedQuery(name="optimisticLock",
  query="SELECT s FROM Student s WHERE s.id LIKE :id",
  lockMode = WRITE)
```

7. **Optimistic Locking Exceptions**

When the persistence provider discovers optimistic locking conflicts on entities, it throws OptimisticLockException. We should be aware that due to the exception the active transaction is always marked for rollback.

It’s good to know how we can react to OptimisticLockException. Conveniently, this exception contains a reference to the conflicting entity. However, it’s not mandatory for the persistence provider to supply it in every situation. There is no guarantee that the object will be available.

There is a recommended way of handling the described exception, though. We should retrieve the entity again by reloading or refreshing, preferably in a new transaction. After that, we can try to update it once more.
