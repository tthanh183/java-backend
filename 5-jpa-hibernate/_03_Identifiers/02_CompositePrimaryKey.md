### Composite Primary Key

1. **Introduction**

In this tutorial, we’ll learn about Composite Primary Keys and the corresponding annotations in JPA.

2. **Composite Primary Keys**

A composite primary key, also called a composite key, is a combination of two or more columns to form a primary key for a table.

In JPA, we have two options to define the composite keys: the @IdClass and @EmbeddedId annotations.

In order to define the composite primary keys, we should follow some rules:

- The composite primary key class must be public.
- It must have a no-arg constructor.
- It must define the equals() and hashCode() methods.
- It must be Serializable.

3. **The IdClass Annotation**

Let’s say we have a table called Account and it has two columns, accountNumber and accountType, that form the composite key. Now we have to map it in JPA.

As per the JPA specification, let’s create an AccountId class with these primary key fields:

```java
public class AccountId implements Serializable {
    private String accountNumber;

    private String accountType;

    // default constructor

    public AccountId(String accountNumber, String accountType) {
        this.accountNumber = accountNumber;
        this.accountType = accountType;
    }

    // equals() and hashCode()
}
```

Next let’s associate the AccountId class with the entity Account.

In order to do that, we need to annotate the entity with the @IdClass annotation. We must also declare the fields from the AccountId class in the entity Account and annotate them with @Id:

```java
@Entity
@IdClass(AccountId.class)
public class Account {
    @Id
    private String accountNumber;

    @Id
    private String accountType;

    // other fields, getters and setters
}
```

4. **The EmbeddedId Annotation**

@EmbeddedId is an alternative to the @IdClass annotation.

Let’s consider another example where we have to persist some information of a Book, with title and language as the primary key fields.

In this case, the primary key class, BookId, must be annotated with @Embeddable:

```java
@Embeddable
public class BookId implements Serializable {
    private String title;
    private String language;

    // default constructor

    public BookId(String title, String language) {
        this.title = title;
        this.language = language;
    }

    // getters, equals() and hashCode() methods
}
```

Then we need to embed this class in the Book entity using @EmbeddedId:

```java
@Entity
public class Book {
    @EmbeddedId
    private BookId bookId;

    // constructors, other fields, getters and setters
}
```

5. **@IdClass vs @EmbeddedId**:

As we can see, the difference on the surface between these two is that with @IdClass we had to specify the columns twice, once in AccountId and again in Account; however, with @EmbeddedId we didn’t.

There are some other trade offs though.

For example, these different structures affect the JPQL queries that we write.

With @IdClass, the query is a bit simpler:

```
SELECT account.accountNumber FROM Account account
```

With @EmbeddedId, we have to do one extra traversal:

```
SELECT book.bookId.title FROM Book book
```

Also, @IdClass can be quite useful in places where we are using a composite key class that we can’t modify.

If we’re going to access parts of the composite key individually, we can make use of @IdClass, but in places where we frequently use the complete identifier as an object, @EmbeddedId is preferred.

6. **Count Query With Composite Primary Key**

We can use JPQL and CriteriaQuery to count the entity using embedded ID as the primary key.

- Using JPQL:

We can write a JPQL query to perform a count on an entity and execute it using the entity manager. getSingleResult() method returns the count for the book entity:

```java
long countBookByEmbeddedIdUsingJPQL() {
    String jpql = "SELECT count(b) FROM Book b";
    Query query = em.createQuery(jpql);
    return (long) query.getSingleResult();
}
```

We can verify the result with a unit test:

```java
@Test
public void givenBookWithEmbeddedId_whenCountCalled_thenReturnsCount() {
    IntStream.rangeClosed(1, 10)
      .forEach(i -> {
          BookId bookId = new BookId("Book" + i, "English");
          Book book = new Book(bookId);
          book.setDescription("Novel and Historical Fiction" + i);
          persist(book);
      });
    assertEquals(10, countBookByEmbeddedIdUsingJPQL());
    assertEquals(10, countBookByEmbeddedIdUsingCriteriaQuery());
}
```

- Using CriteriaQuery:

We can use CriteriaQuery to count the entity using the embedded ID. First, we build the root query and then use getSingleResult() to return the count for the root query:

```java
long countBookByEmbeddedIdUsingCriteriaQuery() {
    CriteriaBuilder criteriaBuilder = em.getCriteriaBuilder();
    CriteriaQuery<Long> criteriaQuery = criteriaBuilder.createQuery(Long.class);
    Root<Book> root = criteriaQuery.from(Book.class);
    criteriaQuery.select(criteriaBuilder.count(root));

    return em.createQuery(criteriaQuery).getSingleResult();
}
```

We can verify the result with a unit test:

```java
@Test
public void givenBookWithEmbeddedId_whenCountCalled_thenReturnsCount() {
    IntStream.rangeClosed(1, 10)
      .forEach(i -> {
          BookId bookId = new BookId("Book" + i, "English");
          Book book = new Book(bookId);
          book.setDescription("Novel and Historical Fiction" + i);
          persist(book);
      });
    assertEquals(10, countBookByEmbeddedIdUsingJPQL());
    assertEquals(10, countBookByEmbeddedIdUsingCriteriaQuery());
}
```
