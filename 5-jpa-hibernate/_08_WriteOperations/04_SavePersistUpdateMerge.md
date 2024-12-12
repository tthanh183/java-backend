### Hibernate: save, persist, update, merge, saveOrUpdate

1. **Introduction**

In this tutorial, we’ll discuss the differences between several methods of the Session interface: save, persist, update, merge, saveOrUpdate, refresh, and replicate.

This isn’t an introduction to Hibernate, and we should already know the basics of configuration, object-relational mapping, and working with entity instances. For an introductory article to Hibernate, visit our tutorial on Hibernate 4 with Spring.

2. **Session as a Persistence Context Implementation**

The Session interface has several methods that eventually result in saving data to the database: persist, save, update, merge, and saveOrUpdate. To understand the difference between these methods, we must first discuss the purpose of the Session as a persistence context, and the difference between the states of entity instances in relation to the Session.

We should also understand the development history of Hibernate that led to some partly duplicated API methods.

- Managing Entity Instances

Apart from object-relational mapping itself, one of the problems that Hibernate solves is the problem of managing entities during runtime. The notion of “persistence context” is Hibernate’s solution to this problem. We can think of persistence context as a container or first-level cache for all the objects that we loaded or saved to a database during a session.

The session is a logical transaction, with boundaries defined by the application’s business logic. When we work with the database through a persistence context, and all of our entity instances are attached to this context, we should always have a single instance of entity for every database record that we interact with during the session.

In Hibernate, the persistence context is represented by the org.hibernate.Session instance. For JPA, it’s the jakarta.persistence.EntityManager. When we use Hibernate as a JPA provider, and operate via the EntityManager interface, the implementation of this interface basically wraps the underlying Session object. However, Hibernate Session provides a richer interface with more possibilities, so sometimes it’s useful to work with Session directly.

- States of Entity Instances

Any entity instance in our application appears in one of the three main states in relation to the Session persistence context:  
    - transient: This instance isn’t, and never was, attached to a Session. This instance has no corresponding rows in the database; it’s usually just a new object that we created to save to the database.  
    - persistent: This instance is associated with a unique Session object. Upon flushing the Session to the database, this entity is guaranteed to have a corresponding consistent record in the database.  
    - detached: This instance was once attached to a Session (in a persistent state), but now it’s not. An instance enters this state if we evict it from the context, clear or close the Session, or put the instance through serialization/deserialization process.  

Here’s a simplified state diagram with comments on Session methods that make the state transitions happen:

![Hibernate Entity State Diagram](https://www.baeldung.com/wp-content/uploads/2016/07/session-state-diagram.png)

When the entity instance is in the persistent state, all the changes that we make to the mapped fields of this instance will be applied to the corresponding database records and fields upon flushing the Session. The persistent instance is “online,” whereas the detached instance is “offline” and is not monitored for changes.

This means that when we change the fields of a persistent object, we don’t have to call save, update, or any of those methods to get these changes to the database. All we need to do is commit the transaction, flush the session, or close the session.

- Conformity to JPA Specification

Hibernate was the most successful Java ORM implementation. As such, the Hibernate API heavily influenced the specifications for the Java persistence API (JPA). Unfortunately, there were also many differences, some major and some more subtle.

To act as an implementation of the JPA standard, Hibernate APIs had to be revised. To match the EntityManager interface, several methods were added to the Session interface. These methods serve the same purpose as the original methods, but conform to the specification, and thus have some differences.

3. **Differences Between the Operations**

It’s important to understand from the beginning that all of the methods (persist, save, update, merge, saveOrUpdate) don’t immediately result in the corresponding SQL UPDATE or INSERT statements. The actual saving of data to the database occurs upon committing the transaction or flushing the Session.


The mentioned methods basically manage the state of entity instances by transitioning them between different states along the lifecycle.

As an example, we’ll use a simple annotation-mapped entity, Person:

```java
@Entity
public class Person {

    @Id
    @GeneratedValue
    private Long id;

    private String name;

    // ... getters and setters

}
```

- Persist

The persist method is intended to add a new entity instance to the persistence context, i.e. transitioning an instance from a transient to persistent state.

We usually call it when we want to add a record to the database (persist an entity instance):

```java
Person person = new Person();
person.setName("John");
session.persist(person);
```

What happens after we call the persist method? The person object has transitioned from a transient to persistent state. The object is in the persistence context now, but not yet saved to the database. The generation of INSERT statements will occur only upon committing the transaction, or flushing or closing the session.

Notice that the persist method has a void return type. It operates on the passed object “in place,” changing its state. The person variable references the actual persisted object.

This method is a later addition to the Session interface. The main differentiating feature of this method is that it conforms to the JSR-220 specification (EJB persistence). We strictly define the semantics of this method in the specification, which basically states that a transient instance becomes persistent (and the operation cascades to all of its relations with cascade=PERSIST or cascade=ALL):  
    - if an instance is already persistent, then this call has no effect for this particular instance (but it still cascades to its relations with cascade=PERSIST or cascade=ALL).  
    - if an instance is detached, we’ll get an exception, either upon calling this method, or upon committing or flushing the session.

Notice that there’s nothing here that concerns the identifier of an instance. The spec doesn’t state that the id will generate right away, regardless of the id generation strategy. The specification for the persist method allows the implementation to issue statements to generate the id on commit or flush. The id won’t necessarily be non-null after we call this method, so we shouldn’t rely upon it.

We may call this method on an already persistent instance, and nothing happens. But if we try to persist a detached instance, the implementation will throw an exception. In the following example, we’ll persist the entity, evict it from the context so it becomes detached, and then try to persist again. The second call to session.persist() causes an exception, so the following code won’t work:

```
Person person = new Person();
person.setName("John");
session.persist(person);

session.evict(person);

session.persist(person); // PersistenceException!
```

- Save

The save method is an “original” Hibernate method that doesn’t conform to the JPA specification.

Its purpose is basically the same as persist, but it has different implementation details. The documentation for this method strictly states that it persists the instance, “first assigning a generated identifier.” The method will return the Serializable value of this identifier:

```
Person person = new Person();
person.setName("John");
Long id = (Long) session.save(person);
```

The effect of saving an already persisted instance is the same as with persist. The difference comes when we try to save a detached instance:

```
Person person = new Person();
person.setName("John");
Long id1 = (Long) session.save(person);

session.evict(person);
Long id2 = (Long) session.save(person);
```

The id2 variable will differ from id1. The save call on a detached instance creates a new persistent instance and assigns it a new identifier, which results in a duplicate record in the database upon committing or flushing

- Merge

The main intention of the merge method is to update a persistent entity instance with new field values from a detached entity instance.

For instance, suppose we have a RESTful interface with a method for retrieving a JSON-serialized object by its id to the caller, and a method that receives an updated version of this object from the caller. An entity that passed through such serialization/deserialization will appear in a detached state.

After deserializing this entity instance, we need to get a persistent entity instance from a persistence context and update its fields with new values from this detached instance. So the merge method does exactly that:  
    - finds an entity instance by id taken from the passed object (either an existing entity instance from the persistence context is retrieved, or a new instance loaded from the database)  
    - copies the state of the passed object to the found instance  
    - returns a newly updated instance

In the following example, we evict (detach) the saved entity from the context, change the name field, and then merge the detached entity:

```
Person person = new Person(); 
person.setName("John"); 
session.save(person);

session.evict(person);
person.setName("Mary");

Person mergedPerson = (Person) session.merge(person);
```

Note that the merge method returns an object. It’s the mergedPerson object we loaded into the persistence context and updated, not the person object that we passed as an argument. They’re two different objects, and we usually need to discard the person object.

As with the persist method, the merge method is specified by JSR-220 to have certain semantics that we can rely upon:  
    - if the entity is detached, it copies upon an existing persistent entity.  
    - if the entity is transient, it copies upon a newly created persistent entity.  
    - this operation cascades for all relations with cascade=MERGE or cascade=ALL mapping.  
    - if the entity is persistent, then this method call doesn’t have an effect on it (but the cascading still takes place).  

- Update

As with persist and save, the update method is an “original” Hibernate method. Its semantics differ in several key points:  
    - it acts upon a passed object (its return type is void). The update method transitions the passed object from a detached to persistent state.  
    - this method throws an exception if we pass it a transient entity.  

In the following example, we save the object, evict (detach) it from the context, and then change its name and call update. Notice that we don’t put the result of the update operation in a separate variable because the update takes place on the person object itself. Basically, we’re reattaching the existing entity instance to the persistence context, something the JPA specification doesn’t allow us to do:

```
Person person = new Person();
person.setName("John");
session.save(person);
session.evict(person);

person.setName("Mary");
session.update(person);
```

Trying to call update on a transient instance will result in an exception. The following won’t work:

```
Person person = new Person();
person.setName("John");
session.update(person); // PersistenceException!
```

- saveOrUpdate

This method appears only in the Hibernate API and doesn’t have its standardized counterpart. Similar to update, we can also use it for reattaching instances.

Actually, the internal DefaultUpdateEventListener class that processes the update method is a subclass of DefaultSaveOrUpdateListener, just overriding some functionality. The main difference of the saveOrUpdate method is that it doesn’t throw an exception when applied to a transient instance, instead it makes this transient instance persistent. The following code will persist a newly created instance of Person:

```
Person person = new Person();
person.setName("John");
session.saveOrUpdate(person);
```

We can think of this method as a universal tool for making an object persistent regardless of its state, whether it’s transient or detached.

- refresh

The refresh method appears in both the Hibernate API and JPA. As the name suggests, the refresh method is called to refresh an entity in the persistent state, thereby synchronizing it with the row in the database.

A call to the refresh method overwrites any changes made to the entity. This is useful when database triggers are used to initialize some of the properties of the object. Let’s see a quick code sample that will refresh a newly saved instance of Person:

```
Person person = new Person();
person.setName("John");
session.save(person);
session.flush();
session.refresh(person);
```

This will reload the person object and all its associated objects and collections so that any modifications triggered after flushing the saved object are reloaded.

It’s important to note how refresh is different from merge: refresh refreshes the entity inside the persistence context with the version in the database, whereas merge updates the entity inside the database with the version in the persistence context.

- replicate

The replicate method is available only in the Hibernate API and is not part of the JPA’s EntityManager interface. We should also note that this method has been deprecated since Hibernate version 6.0 with no replacement.

Sometimes, we may need to load the graph of a persistent instance and replicate it into another datastore without regenerating identifier values. This is where the replicate method comes into play:

```
Session session1 = factory1.openSession();
Person p = session1.get(Person.class, id);
Session session2 = factory2.openSession();
session2.replicate(p, ReplicationMode.LATEST_VERSION);
```

The ReplicationMode determines how replicate will deal with conflicts with existing rows in the database:  
    - ReplicationMode.IGNORE: ignores the object when there is an existing database row with the same identifier.  
    - ReplicationMode.OVERWRITE: overwrites any existing database row with the same identifier.  
    - ReplicationMode.EXCEPTION: throws an exception if there is an existing database row with the same identifier.  
    - ReplicationMode.LATEST_VERSION: overwrites the row if its version number is earlier than the version number of the object, and ignores the object otherwise.  


4. **What to Use**

If we don’t have any special requirements, we should stick to the persist and merge methods because they’re standardized and will conform to the JPA specification.

They’re also portable in case we decide to switch to another persistence provider; however, they may sometimes seem not as useful as the “original” Hibernate methods, save, update, and saveOrUpdate.

5. **Deprecated Methods**

Starting with hibernate 6 the following methods was marked as deprecated and is not encouraged to use:




