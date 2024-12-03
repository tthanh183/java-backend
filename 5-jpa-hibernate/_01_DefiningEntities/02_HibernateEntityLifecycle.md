### Hibernate Entity Lifecycle

1. **Overview**:

Every Hibernate entity naturally has a lifecycle within the framework – it’s either in a transient, managed, detached or deleted state.

Understanding these states on both conceptual and technical level is essential to be able to use Hibernate properly.

To learn about various Hibernate methods that deal with entities, have a look at one of our previous tutorials.

2. **Helper Methods**:


Throughout this tutorial, we’ll consistently use several helper methods:

- HibernateLifecycleUtil.getManagedEntities(session) – we’ll use it to get all managed entities from a Session’s internal store
- DirtyDataInspector.getDirtyEntities() – we’re going to use this method to get a list of all entities that were marked as ‘dirty’
- HibernateLifecycleUtil.queryCount(query) – a convenient method to do count(*) query against the embedded database
All of the above helper methods are statically imported for better readability. You can find their implementations in the GitHub project linked at the end of this article.

3. **It’s All About Persistence Context**:

Before getting into the topic of entity lifecycle, first, we need to understand the persistence context.

Simply put, the persistence context sits between client code and data store. It’s a staging area where persistent data is converted to entities, ready to be read and altered by client code.

Theoretically speaking, the persistence context is an implementation of the Unit of Work pattern. It keeps track of all loaded data, tracks changes of that data, and is responsible to eventually synchronize any changes back to the database at the end of the business transaction.

JPA EntityManager and Hibernate’s Session are an implementation of the persistence context concept. Throughout this article, we’ll use Hibernate Session to represent persistence context.

Hibernate entity lifecycle state explains how the entity is related to a persistence context, as we’ll see next.

> [!NOTE]  
> the EntityManager is essentially an abstraction that wraps around the Hibernate Session when Hibernate is used as the JPA provider. Actions performed via the EntityManager are internally delegated to the corresponding methods of the Hibernate Session. While EntityManager follows the JPA standard, making it portable across different JPA implementations, its underlying behavior relies on the Hibernate Session when Hibernate is the provider.

4. **Managed Entity**:

A managed entity is a representation of a database table row (although that row doesn’t have to exist in the database yet).

This is managed by the currently running Session, and every change made on it will be tracked and propagated to the database automatically.

The Session either loads the entity from the database or re-attaches a detached entity. We’ll discuss detached entities in section 5.

Let’s observe some code to get clarification.

Our sample application defines one entity, the FootballPlayer class. At startup, we’ll initialize the data store with some sample data:

| name              | id |
|-------------------|----|
| Cristiano Ronaldo | 1  |
| Lionel Messi      | 2  |
| Gigi Buffon       | 3  | 

Let’s say we want to change the name of Buffon to start with – we want to put in his full name Gianluigi Buffon instead of Gigi Buffon.

First, we need to start our unit of work by obtaining a Session:

```java
Session session = sessionFactory.openSession();
```

In a server environment, we may inject a Session to our code via a context-aware proxy. The principle remains the same: we need a Session to encapsulate the business transaction of our unit of work.

Next, we’ll instruct our Session to load the data from the persistent store:

```
assertThat(getManagedEntities(session)).isEmpty();

List<FootballPlayer> players = s.createQuery("from FootballPlayer").getResultList();

assertThat(getManagedEntities(session)).size().isEqualTo(3);
```

When we first obtain a Session, its persistent context store is empty, as shown by our first assert statement.

Next, we’re executing a query which retrieves data from the database, creates entity representation of the data, and finally returns the entity for us to use.

Internally, the Session keeps track of all entities it loads in the persistent context store. In our case, the Session’s internal store will contain 3 entities after the query.

Now let’s change Gigi’s name:

```
Transaction transaction = session.getTransaction();
transaction.begin();

FootballPlayer gigiBuffon = players.stream()
  .filter(p -> p.getId() == 3)
  .findFirst()
  .get();

gigiBuffon.setName("Gianluigi Buffon");
transaction.commit();

assertThat(getDirtyEntities()).size().isEqualTo(1);
assertThat(getDirtyEntities().get(0).getName()).isEqualTo("Gianluigi Buffon");
```

On call to transaction commit() or flush(), the Session will find any dirty entities from its tracking list and synchronize the state to the database.

Notice that we didn’t need to call any method to notify Session that we changed something in our entity – since it’s a managed entity, all changes are propagated to the database automatically.

A managed entity is always a persistent entity – it must have a database identifier, even though the database row representation is not yet created i.e. the INSERT statement is pending the end of the unit of work.

See the chapter about transient entities below.

5. **Detached Entity**:

A detached entity is just an ordinary entity POJO whose identity value corresponds to a database row. The difference from a managed entity is that it’s not tracked anymore by any persistence context.

An entity can become detached when the Session used to load it was closed, or when we call Session.evict(entity) or Session.clear().

Let’s see it in the code:

```
FootballPlayer cr7 = session.get(FootballPlayer.class, 1L);

assertThat(getManagedEntities(session)).size().isEqualTo(1);
assertThat(getManagedEntities(session).get(0).getId()).isEqualTo(cr7.getId());

session.evict(cr7);

assertThat(getManagedEntities(session)).size().isEqualTo(0);
```

Our persistence context will not track the changes in detached entities:

```
cr7.setName("CR7");
transaction.commit();

assertThat(getDirtyEntities()).isEmpty();
```

Session.merge(entity)/Session.update(entity) can (re)attach a session:

```
FootballPlayer messi = session.get(FootballPlayer.class, 2L);

session.evict(messi);
messi.setName("Leo Messi");
transaction.commit();

assertThat(getDirtyEntities()).isEmpty();

transaction = startTransaction(session);
session.update(messi);
transaction.commit();

assertThat(getDirtyEntities()).size().isEqualTo(1);
assertThat(getDirtyEntities().get(0).getName()).isEqualTo("Leo Messi");
```

For reference on both Session.merge() and Session.update() see here.

The Identity Field Is All That Matters:

Let’s have a look at the following logic:

```
FootballPlayer gigi = new FootballPlayer();
gigi.setId(3);
gigi.setName("Gigi the Legend");
session.update(gigi);
``` 

> [!NOTE]  
> When you use session.update() to re-attach a detached entity, Hibernate synchronizes it with the database based on its ID. However, if the detached entity only has partial data (e.g., only updated fields with other fields left null), Hibernate treats it as a complete state and overwrites the missing fields with null in the database.

6. **Transient Entity**:

A transient entity is simply an entity object that has no representation in the persistent store and is not managed by any Session.

A typical example of a transient entity would be instantiating a new entity via its constructor.

To make a transient entity persistent, we need to call Session.save(entity) or Session.saveOrUpdate(entity):

```
FootballPlayer neymar = new FootballPlayer();
neymar.setName("Neymar");
session.save(neymar);

assertThat(getManagedEntities(session)).size().isEqualTo(1);
assertThat(neymar.getId()).isNotNull();

int count = queryCount("select count(*) from Football_Player where name='Neymar'");

assertThat(count).isEqualTo(0);

transaction.commit();
count = queryCount("select count(*) from Football_Player where name='Neymar'");

assertThat(count).isEqualTo(1);
```

As soon as we execute Session.save(entity), the entity is assigned an identity value and becomes managed by the Session. However, it might not yet be available in the database as the INSERT operation might be delayed until the end of the unit of work.

7. **Deleted Entity**:

An entity is in a deleted (removed) state if Session.delete(entity) has been called, and the Session has marked the entity for deletion. The DELETE command itself might be issued at the end of the unit of work.

Let’s see it in the following code:

```
session.delete(neymar);

assertThat(getManagedEntities(session).get(0).getStatus()).isEqualTo(Status.DELETED);
```

However, notice that the entity stays in the persistent context store until the end of the unit of work.

