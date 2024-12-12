### Deleting Objects

1. **Overview**

As a full-featured ORM framework, Hibernate is responsible for lifecycle management of persistent objects (entities), including CRUD operations such as read, save, update and delete.

In this article, we explore various ways in which objects may be deleted from a database using Hibernate and we explain common issues and pitfalls that may occur.

We use JPA and only step back and use the Hibernate native API for those features that are not standardized in JPA.

2. **Different Ways of Deleting Objects**


Objects may be deleted in the following scenarios:

- By using EntityManager.remove
- When a deletion is cascaded from other entity instances
- When an orphanRemoval is applied
- By executing a delete JPQL statement
- By executing native queries
- By applying a soft deletion technique (filtering soft-deleted entities by a condition in an @Where clause)

In the remainder of the article, we look at these points in detail.

3. **Deletion Using the Entity Manager**

Deletion with the EntityManager is the most straightforward way to remove an entity instance:

```
Foo foo = new Foo("foo");
entityManager.persist(foo);
flushAndClear();

foo = entityManager.find(Foo.class, foo.getId());
assertThat(foo, notNullValue());
entityManager.remove(foo);
flushAndClear();

assertThat(entityManager.find(Foo.class, foo.getId()), nullValue());

```

In the examples in this article we use a helper method to flush and clear the persistence context when needed:

```java
void flushAndClear() {
    entityManager.flush();
    entityManager.clear();
}
```

After calling the EntityManager.remove method, the supplied instance transitions to the removed state and the associated deletion from the database occurs on the next flush.

Note that deleted instance is re-persisted if a PERSIST operation is applied to it. A common mistake is to ignore that a PERSIST operation has been applied to a removed instance (usually, because it is cascaded from another instance at the flush time), because the section 3.2.2 of the JPA specification mandates that such instance is to be persisted again in such a case.

We illustrate this by defining a @ManyToOne association from Foo to Bar:

```java
@Entity
public class Foo {
    @ManyToOne(fetch = FetchType.LAZY, cascade = CascadeType.ALL)
    private Bar bar;

    // other mappings, getters and setters
}   
    
```

When we delete a Bar instance referenced by a Foo instance which is also loaded in the persistence context, the Bar instance will not be removed from the database:

```
Bar bar = new Bar("bar");
Foo foo = new Foo("foo");
foo.setBar(bar);
entityManager.persist(foo);
flushAndClear();

foo = entityManager.find(Foo.class, foo.getId());
bar = entityManager.find(Bar.class, bar.getId());
entityManager.remove(bar);
flushAndClear();

bar = entityManager.find(Bar.class, bar.getId());
assertThat(bar, notNullValue());

foo = entityManager.find(Foo.class, foo.getId());
foo.setBar(null);
entityManager.remove(bar);
flushAndClear();

assertThat(entityManager.find(Bar.class, bar.getId()), nullValue());
```

If the removed Bar is referenced by a Foo, the PERSIST operation is cascaded from Foo to Bar because the association is marked with cascade = CascadeType.ALL and the deletion is unscheduled. To verify that this is happening, we may enable trace log level for the org.hibernate package and search for entries such as un-scheduling entity deletion.

4. **Cascaded Deletion**

Deletion can be cascaded to children entities when parents are removed:

```
Bar bar = new Bar("bar");
Foo foo = new Foo("foo");
foo.setBar(bar);
entityManager.persist(foo);
flushAndClear();

foo = entityManager.find(Foo.class, foo.getId());
entityManager.remove(foo);
flushAndClear();

assertThat(entityManager.find(Foo.class, foo.getId()), nullValue());
assertThat(entityManager.find(Bar.class, bar.getId()), nullValue());
```

Here bar is removed because removal is cascaded from foo, as the association is declared to cascade all life cycle operations from Foo to Bar.

Note that it is almost always a bug to cascade REMOVE operation in a @ManyToMany association, because that would trigger removing child instances which may be associated with other parent instances. This also applies to CascadeType.ALL, as it means that all operations are to be cascaded, including the REMOVE operation.

5. **Removal of Orphans**

The orphanRemoval directive declares that associated entity instances are to be removed when they are disassociated from the parent, or equivalently when the parent is removed.

We show this by defining such an association from Bar to Baz:

```java
@Entity
public class Bar {
    @OneToMany(cascade = CascadeType.ALL, orphanRemoval = true)
    private List<Baz> bazList = new ArrayList<>();

    // other mappings, getters and setters
}
```

Then a Baz instance is deleted automatically when it is removed from the list of a parent Bar instance:

```
Bar bar = new Bar("bar");
Baz baz = new Baz("baz");
bar.getBazList().add(baz);
entityManager.persist(bar);
flushAndClear();

bar = entityManager.find(Bar.class, bar.getId());
baz = bar.getBazList().get(0);
bar.getBazList().remove(baz);
flushAndClear();

assertThat(entityManager.find(Baz.class, baz.getId()), nullValue());
```

The semantics of the orphanRemoval operation is completely similar to a REMOVE operation applied directly to the affected child instances, which means that the REMOVE operation is further cascaded to nested children. As a consequence, you have to ensure that no other instances reference the removed ones (otherwise they are re-persisted).

6. **Deletion Using a JPQL Statement**

Hibernate supports DML-style delete operations:

```
Foo foo = new Foo("foo");
entityManager.persist(foo);
flushAndClear();

entityManager.createQuery("delete from Foo where id = :id")
  .setParameter("id", foo.getId())
  .executeUpdate();

assertThat(entityManager.find(Foo.class, foo.getId()), nullValue());
```

It is important to note that DML-style JPQL statements affect neither the state nor life cycle of entity instances that are already loaded into the persistence context, so it is recommended that they are executed prior to loading the affected entities.

7. **Deletion Using Native Queries**

Sometimes we need to fall back to native queries to achieve something that is not supported by Hibernate or is specific to a database vendor. We may also delete data in the database with native queries:

```
Foo foo = new Foo("foo");
entityManager.persist(foo);
flushAndClear();

entityManager.createNativeQuery("delete from FOO where ID = :id")
  .setParameter("id", foo.getId())
  .executeUpdate();

assertThat(entityManager.find(Foo.class, foo.getId()), nullValue());
```

The same recommendation applies to native queries as for JPA DML-style statements, i.e. native queries affect neither the state nor life cycle of entity instances which are loaded into the persistence context prior to execution of the queries.

8. **Soft Deletion**

Often it is not desirable to remove data from a database because of auditing purposes and keeping history. In such situations, we may apply a technique called soft deletes. Basically, we just mark a row as removed and we filter it out when retrieving data.

In order to avoid lots of redundant conditions in where clauses in all the queries that read soft-deletable entities, Hibernate provides the @Where annotation which can be placed on an entity and which contains an SQL fragment that is automatically added to SQL queries generated for that entity.

To demonstrate this, we add the @Where annotation and a column named DELETED to the Foo entity:

```java
@Entity
@Where(clause = "DELETED = 0")
public class Foo {
    // other mappings

    @Column(name = "DELETED")
    private Integer deleted = 0;
    
    // getters and setters

    public void setDeleted() {
        this.deleted = 1;
    }
}
```

The following test confirms that everything works as expected:

```
Foo foo = new Foo("foo");
entityManager.persist(foo);
flushAndClear();

foo = entityManager.find(Foo.class, foo.getId());
foo.setDeleted();
flushAndClear();

assertThat(entityManager.find(Foo.class, foo.getId()), nullValue());
```