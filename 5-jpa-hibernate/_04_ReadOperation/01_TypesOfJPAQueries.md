### Types of JPA Queries

1. **Overview**

In this tutorial, we’ll discuss the different types of JPA queries. Moreover, we’ll focus on comparing the differences between them and expanding on each one’s pros and cons.

2. **Setup**

Firstly, let’s define the UserEntity class we’ll use for all examples in this article:

```java
@Table(name = "users")
@Entity
public class UserEntity {

    @Id
    private Long id;
    private String name;
    //Standard constructor, getters and setters.

}
```

There are three basic types of JPA Queries:
- Query, written in Java Persistence Query Language (JPQL) syntax.
- NativeQuery, written in plain SQL syntax.
- Criteria API Query, constructed programmatically via different methods.

Let’s explore them.

3. **Query**

A Query is similar in syntax to SQL, and it’s generally used to perform CRUD operations:

```java
public UserEntity getUserByIdWithPlainQuery(Long id) {
    Query jpqlQuery = getEntityManager().createQuery("SELECT u FROM UserEntity u WHERE u.id=:id");
    jpqlQuery.setParameter("id", id);
    return (UserEntity) jpqlQuery.getSingleResult();
}
```

This Query retrieves the matching record from the users table and also maps it to the UserEntity object.

There are two additional Query sub-types:
- TypedQuery, which is used when we know the type of the result.
- NamedQuery, which is a predefined query that is declared in the entity class.

Let’s see them in action.

- TypedQuery:

We need to pay attention to the return statement in our previous example. JPA can’t deduce what the Query result type will be, and, as a result, we have to cast.

But, JPA provides a special Query sub-type known as a TypedQuery. This is always preferred if we know our Query result type beforehand. Additionally, it makes our code much more reliable and easier to test.

Let’s see a TypedQuery alternative, compared to our first example:

```java
public UserEntity getUserByIdWithTypedQuery(Long id) {
    TypedQuery<UserEntity> typedQuery
      = getEntityManager().createQuery("SELECT u FROM UserEntity u WHERE u.id=:id", UserEntity.class);
    typedQuery.setParameter("id", id);
    return typedQuery.getSingleResult();
}
```

This way, we get stronger typing for free, avoiding possible casting exceptions down the road.

- NamedQuery:

While we can dynamically define a Query on specific methods, they can eventually grow into a hard-to-maintain codebase. What if we could keep general usage queries in one centralized, easy-to-read place?

JPA’s also got us covered on this with another Query sub-type known as a NamedQuery.

We can define NamedQueries in orm.xml or a properties file.

Also, we can define NamedQuery on the Entity class itself, providing a centralized, quick and easy way to read and find an Entity‘s related queries.

All NamedQueries must have a unique name.

Let’s see how we can add a NamedQuery to our UserEntity class:

```java
@Table(name = "users")
@Entity
@NamedQuery(name = "UserEntity.findByUserId", query = "SELECT u FROM UserEntity u WHERE u.id=:userId")
public class UserEntity {

    @Id
    private Long id;
    private String name;
    //Standard constructor, getters and setters.

}
```

The @NamedQuery annotation has to be grouped inside a @NamedQueries annotation if we’re using Java before version 8. From Java 8 forward, we can simply repeat the @NamedQuery annotation at our Entity class.

Using a NamedQuery is very simple:

```java
public UserEntity getUserByIdWithNamedQuery(Long id) {
    Query namedQuery = getEntityManager().createNamedQuery("UserEntity.findByUserId");
    namedQuery.setParameter("userId", id);
    return (UserEntity) namedQuery.getSingleResult();
}
```

4. **NativeQuery**

A NativeQuery is simply an SQL query. These allow us to unleash the full power of our database, as we can use proprietary features not available in JPQL-restricted syntax.

This comes at a cost. We lose the database portability of our application with NativeQuery because our JPA provider can’t abstract specific details from the database implementation or vendor anymore.

Let’s see how to use a NativeQuery that yields the same results as our previous examples:

```java
public UserEntity getUserByIdWithNativeQuery(Long id) {
    Query nativeQuery
      = getEntityManager().createNativeQuery("SELECT * FROM users WHERE id=:userId", UserEntity.class);
    nativeQuery.setParameter("userId", id);
    return (UserEntity) nativeQuery.getSingleResult();
}
```

We must always consider if a NativeQuery is the only option. Most of the time, a good JPQL Query can fulfill our needs and most importantly, maintain a level of abstraction from the actual database implementation.

Using NativeQuery doesn’t necessarily mean locking ourselves to one specific database vendor. After all, if our queries don’t use proprietary SQL commands and are using only a standard SQL syntax, switching providers should not be an issue.

5. **Query, NamedQuery, and NativeQuery**

So far, we’ve learned about Query, NamedQuery, and NativeQuery.

Now, let’s revisit them quickly and summarize their pros and cons.

- Query:

We can create a query using entityManager.createQuery(queryString).

Next, let’s explore the pros and cons of Query:

Pros:
    - When we create a query using EntityManager, we can build dynamic query strings
    - Queries are written in JPQL, so they’re portable
Cons:
    - For a dynamic query, it may be compiled into a native SQL statement multiple times depending on the query plan cache
    - The queries may scatter into various Java classes and they’re mixed in with Java code. Therefore, it could be difficult to maintain if a project contains many queries

- NamedQuery:

Once a NamedQuery has been defined, we can reference it using the EntityManager:

> entityManager.createNamedQuery(queryName);

Now, let’s look at the advantages and disadvantages of NamedQueries:

Pros:
    - NamedQueries are compiled and validated when the persistence unit is loaded. That is to say, they’re compiled only once
    - We can centralize NamedQueries to make them easier to maintain – for example, in orm.xml, in properties files, or on @Entity classes

Cons:
    - NamedQueries are always static.
    - NamedQueries can be referenced in Spring Data JPA repositories. However, dynamic sorting is not supported

- NativeQuery:

We can create a NativeQuery using EntityManager:

> entityManager.createNativeQuery(queryString);

Depending on the result mapping, we can also pass the second parameter to the method, such as an Entity class, as we’ve seen in a previous example.

NativeQueries have pros and cons, too. Let’s look at them quickly:

Pros:
    - As our queries get complex, sometimes the JPA-generated SQL statements aren’t the most optimized. In this case, we can use NativeQueries to make the queries more efficient
    - NativeQueries allow us to use database vendor-specific features. Sometimes, those features can give our queries better performance

Cons:
    - Vendor-specific features can bring convenience and better performance, but we pay for that benefit by losing the portability from one database to another

6. **Criteria API Query**

Criteria API queries are programmatically-built, type-safe queries – somewhat similar to JPQL queries in syntax:

```java
public UserEntity getUserByIdWithCriteriaQuery(Long id) {
    CriteriaBuilder criteriaBuilder = getEntityManager().getCriteriaBuilder();
    CriteriaQuery<UserEntity> criteriaQuery = criteriaBuilder.createQuery(UserEntity.class);
    Root<UserEntity> userRoot = criteriaQuery.from(UserEntity.class);
    UserEntity queryResult = getEntityManager().createQuery(criteriaQuery.select(userRoot)
      .where(criteriaBuilder.equal(userRoot.get("id"), id)))
      .getSingleResult();
    return queryResult;
}
```

It can be daunting to use Criteria API queries first-hand, but they can be a great choice when we need to add dynamic query elements or when coupled with the JPA Metamodel.

