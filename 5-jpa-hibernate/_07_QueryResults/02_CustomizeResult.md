### Customize Result of JPA Queries

1. **Overview**

While Spring Data JPA can abstract the creation of queries to retrieve entities from the database in specific situations, we sometimes need to customize our queries, such as when we add aggregation functions.

In this tutorial, we’ll focus on how to convert the results of those queries into an object. We’ll explore two different solutions, one involving the JPA specification and a POJO, and another using Spring Data Projection.

2. **JPA Queries and the Aggregation Problem**

JPA queries typically produce their results as instances of a mapped entity. However, queries with aggregation functions normally return the result as Object[].

To understand the problem, let’s define a domain model based on the relationship between posts and comments:

```java
@Entity
public class Post {
    @Id
    private Integer id;
    private String title;
    private String content;
    @OneToMany(mappedBy = "post")
    private List comments;

    // additional properties
    // standard constructors, getters, and setters
}

@Entity
public class Comment {
    @Id
    private Integer id;
    private Integer year;
    private boolean approved;
    private String content;
    @ManyToOne
    private Post post;

    // additional properties
    // standard constructors, getters, and setters
}
```

Our model defines that a post can have many comments, and each comment belongs to one post. Let’s use a Spring Data Repository with this model:

```java
@Repository
public interface CommentRepository extends JpaRepository<Comment, Integer> {
    // query methods
}
```

Now let’s count the comments grouped by year:

```java
@Query("SELECT c.year, COUNT(c.year) FROM Comment AS c GROUP BY c.year ORDER BY c.year DESC")
List<Object[]> countTotalCommentsByYear();
```

The result of the previous JPA query can’t be loaded into an instance of Comment because the result is a different shape. The year and COUNT specified in the query don’t match our entity object.

While we can still access the results in the general-purpose Object[] returned in the list, doing so will result in messy, error-prone code.

3. **Customizing the Result With Class Constructors**

The JPA specification allows us to customize results in an object-oriented fashion. Therefore, we can use a JPQL constructor expression to set the result:

```java
@Query("SELECT new com.baeldung.aggregation.model.custom.CommentCount(c.year, COUNT(c.year)) "
  + "FROM Comment AS c GROUP BY c.year ORDER BY c.year DESC")
List<CommentCount> countTotalCommentsByYearClass();
```

This binds the output of the SELECT statement to a POJO. The class specified needs to have a constructor that matches the projected attributes exactly, but it’s not required to be annotated with @Entity.

We can also see that the constructor declared in the JPQL must have a fully qualified name:

```java
package com.baeldung.aggregation.model.custom;

public class CommentCount {
    private Integer year;
    private Long total;

    public CommentCount(Integer year, Long total) {
        this.year = year;
        this.total = total;
    }
    // getters and setters
}
```

4. **Customizing the Result With Spring Data Projection**

Another possible solution is to customize the result of JPA queries with Spring Data Projection. This functionality allows us to project query results with considerably less code.

- Customizing the Result of JPA Queries

To use interface-based projection, we must define a Java interface composed of getter methods that match the projected attribute names. Let’s define an interface for our query result:

```java
public interface ICommentCount {
    Integer getYearComment();
    Long getTotalComment();
}
```

Now let’s express our query with the result returned as List<ICommentCount>:

```java
@Query("SELECT c.year AS yearComment, COUNT(c.year) AS totalComment "
  + "FROM Comment AS c GROUP BY c.year ORDER BY c.year DESC")
List<ICommentCount> countTotalCommentsByYearInterface();
```

To allow Spring to bind the projected values to our interface, we need to give aliases to each projected attribute with the property name found in the interface.

Spring Data will then construct the result on-the-fly, and return a proxy instance for each row of the result.

- Customizing the Result of Native Queries

We can face situations where JPA queries aren’t as fast as native SQL, or can’t use specific features of our database engine. To solve this, we’ll use native queries.

One advantage of interface-based projection is that we can use it for native queries. Let’s use ICommentCount again and bind it to a SQL query:

```java
@Query(value = "SELECT c.year AS yearComment, COUNT(c.*) AS totalComment "
  + "FROM comment AS c GROUP BY c.year ORDER BY c.year DESC", nativeQuery = true)
List<ICommentCount> countTotalCommentsByYearNative();
```

This works identically to JPQL queries.
