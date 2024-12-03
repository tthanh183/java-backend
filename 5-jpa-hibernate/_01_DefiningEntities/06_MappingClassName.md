### Mapping Class Name

1. **Introduction**:

In this short tutorial, we’ll learn how to set SQL table names using JPA.

We’ll cover how JPA generates the default names and how to provide custom ones.
2. **Default Table Names**:

The JPA default table name generation is specific to its implementation.

For instance, in Hibernate the default table name is the name of the class with the first letter capitalized. It’s determined through the ImplicitNamingStrategy contract.

But we can change this behaviour by implementing a PhysicalNamingStrategy interface.

3. **Using @Table**:

The easiest way to set a custom SQL table name is to annotate the entity with @jakarta.persistence.Table and define its name parameter:

```java
@Entity
@Table(name = "ARTICLES")
public class Article {
    // ...
}
```

We can also store the table name in a static final variable:

```java 
@Entity
@Table(name = Article.TABLE_NAME)
public class Article {
    public static final String TABLE_NAME= "ARTICLES";
    // ...
}
```

4. **Overwriting the Table Name in JPQL Queries**:

By default in JPQL queries, we use the entity class name:

```
select * from Article
```
But we can change it by defining the name parameter in the @jakarta.persistence.Entity annotation:

```
@Entity(name = "MyArticle")
public class Article {
    @Id
    private Long id;
    private String title;
}
```

Then we’d change our JPQL query to:

```
select * from MyArticle
```

> [!NOTE]  
> The name attribute in @Entity does not affect the table name in the database. You can set the name attribute to a value different from the class name, but this only impacts JPQL queries, not the class name or table name. The table name is controlled by the @Table annotation (or defaults to the class name if @Table is not used).

