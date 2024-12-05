### Java Records

1. **Overview**:

In this tutorial, we’ll explore how to use Java Records with JPA. We’ll start by exploring why records can’t be used at entities.

Then, we’ll see how to use records with JPA. We’ll also look at how to use records with Spring Data JPA in a Spring Boot Application.

2. **Records vs Entities**:

Records are immutable and are used to store data. They contain fields, all-args constructor, getters, toString, and equals/hashCode methods. Since they are immutable, they don’t have setters. Because of their concise syntax, they are often used as data transfer objects (DTOs) in Java applications.

Entities are classes that are mapped to a database table. They are used to represent an entry in a database. Their fields are mapped to columns in the database table.

- Record can’t be an entity:

Entities are handled by the JPA provider. JPA providers are responsible for creating the database tables, mapping the entities to the tables, and persisting the entities to the database. In popular JPA providers like Hibernate, entities are created and managed using proxies.

Proxies are classes that are generated at runtime and extend the entity class. These proxies rely on the entity class to have a no-args constructor and setters. Since records don’t have these, they can’t be used as entities.

- Other ways to use records:

Due to the ease and safety of using records within Java applications, it may be beneficial to use them with JPA in some other ways.

In JPA, we can use records in the following ways:   
    - Convert the results of a query to a record  
    - Use records as DTOs to transfer data between layers  
    - Convert entities to records.  
    - Use records as @IdClass for composite primary keys  

3. **Project Setup**:

We’ll use Spring Boot to create a simple application that uses JPA and Spring Data JPA. Then we’ll look at a few ways to use records while interacting with the database.

- Dependencies:

Let’s start by adding the Spring Data JPA dependency to our project:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
    <version>3.0.4</version>
</dependency>
```

In addition to Spring Data JPA, we’ll also need to configure a database. We can use any SQL database. For example, we can use the in-memory H2 database.

- Entity and Record:

Let’s create an entity that we’ll use to interact with the database. We’ll create a Book entity that will be mapped to a book table in the database:

```java
@Entity
@Table(name = "book")
public class Book {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String title;
    private String author;
    private String isbn;
    
    // constructors, getters, setters
}
```

Let’s also create a Record that corresponds to the Book entity:

```java
public record BookRecord(Long id, String title, String author, String isbn) {}
```

Next, we’ll look at a few ways to use the record instead of the entity in our application.

4. **Using Records with JPA**:

The JPA API provides a few ways to interact with the database in which it is possible to use records. Let’s look at a few of them.

- Criteria Builder:

Let’s start by looking at how to use records with CriteriaBuilder. We’ll make a query that returns all the books in the database:

```java
public class QueryService {
    @PersistenceContext
    private EntityManager entityManager;
    
    public List<BookRecord> findAllBooks() {
        CriteriaBuilder cb = entityManager.getCriteriaBuilder();
        CriteriaQuery<BookRecord> query = cb.createQuery(BookRecord.class);
        Root<Book> root = query.from(Book.class);
        query.select(cb.construct(BookRecord.class, root.get("id"), root.get("title"), 
          root.get("author"), root.get("isbn")));
        return entityManager.createQuery(query).getResultList();
    }
}
```
In the above code, we use CriteriaBuilder to create a CriteriaQuery that returns a BookRecord.

Let’s look at some of the steps in the above code:
    - We create a CriteriaQuery using the CriteriaBuilder.createQuery() method. We pass the class of the record that we want to return as the parameter  
    - We then create a Root using the CriteriaQuery.from() method. We pass the entity class as the parameter. This is how we specify the table that we want to query 
    - Then, we use the CriteriaQuery.select() method to specify a select clause. We use the CriteriaBuilder.construct() method to convert the query results to a record. We pass the class of the record and the fields of the entity that we want to pass to the record constructor as the parameters  
    - Finally, we use the EntityManager.createQuery() method to create a TypedQuery from CriteriaQuery. We then use the TypedQuery.getResultList() method to get the results of the query  

This will create a select query to get all the books in the database. It’ll then convert each result to a BookRecord using the construct() method and return a list of records instead of a list of entities when we call the getResultList() method.

In this way, we can use the entity class to create a query but use records for the rest of the application.

- TypedQuery:

Similar to the CriteriaBuilder, we can use a typed query to return a record instead of an entity. Let’s add a method in our QueryService to get a single book as records using a typed query:

```java 
public BookRecord findBookByTitle(String title) {
    TypedQuery<BookRecord> query = entityManager
      .createQuery("SELECT " +
        "new com.baeldung.recordswithjpa.records.BookRecord(b.id, b.title, b.author, b.isbn) " +
        "FROM Book b WHERE b.title = :title", BookRecord.class);
    query.setParameter("title", title);
    return query.getSingleResult();
}
```

TypedQuery allows us to convert the results of a query into any type as long as the type has a constructor that takes the same number of parameters as the results of the query.

In the above code, we use the EntityManager.createQuery() method to create a TypedQuery. We pass the query string and the class of the record as the parameters. Then, we use the TypedQuery.setParameter() method to set the parameters of the query. Finally, we use the TypedQuery.getSingleResult() method to get the result of the query, which will be a BookRecord object.

- Native Query:

We can also use native queries to get the results of a query as records. However, a native query doesn’t allow us to convert the results into any type. Instead, we need to use a mapping to convert the results into a record. First, let’s define a mapping in our entity:

```java
@SqlResultSetMapping(
  name = "BookRecordMapping",
  classes = @ConstructorResult(
    targetClass = BookRecord.class,
    columns = {
      @ColumnResult(name = "id", type = Long.class),
      @ColumnResult(name = "title", type = String.class),
      @ColumnResult(name = "author", type = String.class),
      @ColumnResult(name = "isbn", type = String.class)
    }
  )
)
@Entity
@Table(name = "book")
public class Book {
    // ...
}
```

The mapping will work in the following way:
    - The name attribute of the @SqlResultSetMapping annotation specifies the name of the mapping.
    - The @ConstructorResult annotation specifies that we want to use the constructor of the record to convert the results.
    - The targetClass attribute of the @ConstructorResult annotation specifies the class of the record.
    - The @ColumnResult annotation specifies the column name and the type of the column. These column values will be passed to the constructor of the record.

We can then use this mapping in our native query to get the results as records:

```java
public List<BookRecord> findAllBooksUsingMapping() {
    Query query = entityManager.createNativeQuery("SELECT * FROM book", "BookRecordMapping");
    return query.getResultList();
}
```
This will create a native query that returns all the books in the database. It’ll convert the results into a BookRecord using the mapping and return a list of records instead of a list of entities when we call the getResultList() method.

5. **Using Records with Spring Data JPA**:

Spring Data JPA provides a few improvements to the JPA API. It enables us to use records with Spring Data JPA repositories in a few ways. Let’s look at how we can use records with Spring Data JPA repositories.

- Auto mapping from Entity to Record:

Spring Data Repositories allow us to use records as the return type of the methods in the repository. This will automatically map the entity to the record. This is only possible if the record has exactly the same fields as the entity. Let’s look at an example:

```java
public interface BookRepository extends JpaRepository<Book, Long> {
    List<BookRecord> findBookByAuthor(String author);
}
```

Since the BookRecord has the same fields as the Book entity, Spring Data JPA will automatically map the entity to the record and return a list of records instead of a list of entities when we call the findBookByAuthor() method.

- Using Record with @Query:

Similar to TypedQuery, we can use records with the @Query annotation in Spring Data JPA repositories. Let’s look at an example:

```java
public interface BookRepository extends JpaRepository<Book, Long> {
    @Query("SELECT new com.baeldung.jpa.records.BookRecord(b.id, b.title, b.author, b.isbn) FROM Book b WHERE b.id = :id")
    BookRecord findBookById(@Param("id") Long id);
}
```

Spring Data JPA will automatically convert the results of the query into a BookRecord and return a single record instead of an entity when we call the findBookById() method.

- Custom Repository Implementation:

In case automatic mapping is not an option, we can also define a custom repository implementation that allows us to define our own mapping. Let’s start by creating a CustomBookRecord class that will be used as the return type of the methods in the repository:

```java
public record CustomBookRecord(Long id, String title) {}
```

Please note that the CustomBookRecord class doesn’t have the same fields as the Book entity. It only has the id and the title fields.

Then, we can create a custom repository implementation that will use the CustomBookRecord class:

```java
public interface CustomBookRepository {
    List<CustomBookRecord> findAllBooks();
}
```

In the implementation of the repository, we can define the methods that will be used to map the results of the queries into the CustomBookRecord class:

```java
@Repository
public class CustomBookRepositoryImpl implements CustomBookRepository {
    private final JdbcTemplate jdbcTemplate;

    public CustomBookRepositoryImpl(JdbcTemplate jdbcTemplate) {
        this.jdbcTemplate = jdbcTemplate;
    }

    public List<CustomBookRecord> findAllBooks() {
        return jdbcTemplate.query("SELECT id, title FROM book", (rs, rowNum) ->
          new CustomBookRecord(rs.getLong("id"), rs.getString("title")));
    }
}
```
In the above code, we use the JdbcTemplate.query() method to execute the query and map the results into a CustomBookRecord using a lambda expression which is an implementation of the RowMapper interface.

6. **Using Records as @Embeddables**:

With the recent updates, Hibernate now supports mapping Java records as embeddable. We can use Java records to represent a group of related properties we want to embed within an entity class:

```java
@Embeddable
public record Author (
    String firstName,
    String lastName
) {}
```
In this example, the Author is a Java record marked as @Embeddable. It has two fields: firstName and lastName. We can use this record in an entity class to represent an author. To use this Record inside our Entity, we should mark it with @Embedded annotation:

```java
@Entity
@Table(name = "embeadable_author_book")
public class EmbeddableBook {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String title;
    @Embedded
    private Author author;
    private String isbn;
    //...
}
```

Hibernate 6 before the 6.2 version requires some additional work to use Records. The Author field would need additional annotation @EmbeddableInstantiator(AuthorInstallator.class), and we should provide the implementation for the EmbeddableInstantiator interface:

```java
public class AuthorInstallator implements EmbeddableInstantiator {

    public boolean isInstance(Object object, SessionFactoryImplementor sessionFactory) {
        return object instanceof Author;
    }

    public boolean isSameClass(Object object, SessionFactoryImplementor sessionFactory) {
        return object.getClass().equals(Author.class);
    }
    
    @Override
    public Object instantiate(final ValueAccess valueAccess, 
      final SessionFactoryImplementor sessionFactoryImplementor) {
        final String firstName = valueAccess.getValue(0, String.class);
        final String secondName = valueAccess.getValue(1, String.class);
        return new Author(firstName, secondName);
    }
}
```

Furthermore, Hibernate also supports using structured SQL types to persist to a struct. The Author record can be marked with @Struct annotation that allows mapping the record to a structured SQL type. This can be useful when we want to map the record to a complex type in the database corresponding to a struct.

7. **Using Records as @IdClass**: 

In JPA, an entity may have a composite primary key. Normally, we use an @IdClass annotation to define a class that will be used as the primary key of the entity. Since the latest Hibernate updates, we can also use records as the @IdClass of an entity.

- Defining an @IdClass Using a Record:

Let’s look at an example:

```java
public record BookId(Long id, Long isbn) {
}
```

In the above code, we define a BookId record that has two fields: id and isbn.

- Using the @IdClass in an Entity

Next, let’s use the BookId record as the @IdClass of the CompositeBook entity:

```java
@Entity
@Table(name = "book")
@IdClass(BookId.class)
public class CompositeBook {
    @Id
    private Long id;
    @Id
    private Long isbn;
    private String title;
    private String author;
    
    // constructors, getters, setters
}
```

In the above code, we use the @IdClass annotation to link the BookId record to the CompositeBook entity. The id and isbn fields are marked with @Id and can be set using a BookId object.

- Using the Record in a Repository:

We can use the BookId record as the primary key in the repository as well:

```java
@Repository
public interface CompositeBookRepository extends JpaRepository<CompositeBook, BookId> {
}
```

In the above code, we use the BookId record as the primary key of the CompositeBook entity in the repository.

- Using Record to Fetch Data:

Since we added the BookId record to the repository, we can use it to fetch or update data.  Let’s write a test to save a CompositeBook object to the database and then fetch it using the BookId record:

```java
@SpringBootTest
public class CompositeBookRepositoryIntegrationTest {
    @Autowired
    private CompositeBookRepository compositeBookRepository;

    @Test
    public void givenCompositeBook_whenSave_thenSaveToDatabase() {
        CompositeBook compositeBook = new CompositeBook(new BookId(1L, 1234567890L),
          "Book Title", "Author Name");
        compositeBookRepository.save(compositeBook);

        CompositeBook savedBook = compositeBookRepository
          .findById(new BookId(1L, 1234567890L))
          .orElse(null);
        assertNotNull(savedBook);
        assertEquals("Book Title", savedBook.getTitle());
        assertEquals("Author Name", savedBook.getAuthor());
    }
}
```

As we can see, we can use the BookId record in the findById() method of the repository to get the CompositeBook object from the database.
