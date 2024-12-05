### Persist Enums 

1. **Overview**:

In JPA version 2.0 and below, there’s no convenient way to map Enum values to a database column. Each option has its limitations and drawbacks. These issues can be avoided by using JPA 2.1 features. In the latest version of JPA 3.1, there were not added significant features of changes to the existing in terms of persisting Enums.

In this tutorial, we’ll take a look at the different possibilities we have to persist enums in a database using JPA. We’ll also describe their advantages and disadvantages as well as provide simple code examples.

2. **Using @Enumerated Annotation**:

use the @Enumerated annotation. This way, we can instruct a JPA provider to convert an enum to its ordinal or String value.

We’ll explore both options in this section.

But let’s first create a simple @Entity that we’ll be using throughout this tutorial:

```java
@Entity
public class Article {
    @Id
    private int id;

    private String title;

    // standard constructors, getters and setters
}
```
- Mapping Ordinal Value

If we put the @Enumerated(EnumType.ORDINAL) annotation on the enum field, JPA will use the Enum.ordinal() value when persisting a given entity in the database.

Let’s introduce the first enum:

```java
public enum Status {
    OPEN, REVIEW, APPROVED, REJECTED;
}
```
Next, let’s add it to the Article class and annotate it with @Enumerated(EnumType.ORDINAL):


```java
@Entity
public class Article {
    @Id
    private int id;

    private String title;

    @Enumerated(EnumType.ORDINAL)
    private Status status;
}
```

Now when persisting an Article entity:

```
Article article = new Article();
article.setId(1);
article.setTitle("ordinal title");
article.setStatus(Status.OPEN);
```

JPA will trigger the following SQL statement:

```
insert 
into
    Article
    (status, title, id) 
values
    (?, ?, ?)
binding parameter [1] as [INTEGER] - [0]
binding parameter [2] as [VARCHAR] - [ordinal title]
binding parameter [3] as [INTEGER] - [1]
```

A problem arises with this kind of mapping when we need to modify our enum. If we add a new value in the middle or rearrange the enum’s order, we’ll break the existing data model.

Such issues might be hard to catch as well as problematic to fix since we would have to update all the database records.

- Mapping String Value

Analogously, JPA will use the Enum.name() value when storing an entity if we annotate the enum field with @Enumerated(EnumType.STRING).

Let’s create the second enum:

```java
public enum Type {
    INTERNAL, EXTERNAL;
}
```

And let’s add it to our Article class and annotate it with @Enumerated(EnumType.STRING):


```java
@Entity
public class Article {
    @Id
    private int id;

    private String title;

    @Enumerated(EnumType.ORDINAL)
    private Status status;

    @Enumerated(EnumType.STRING)
    private Type type;
}
```
Now when persisting an Article entity:

```
Article article = new Article();
article.setId(2);
article.setTitle("string title");
article.setType(Type.EXTERNAL);
```

JPA will execute the following SQL statement:

```
insert 
into
    Article
    (status, title, type, id) 
values
    (?, ?, ?, ?)
binding parameter [1] as [INTEGER] - [null]
binding parameter [2] as [VARCHAR] - [string title]
binding parameter [3] as [VARCHAR] - [EXTERNAL]
binding parameter [4] as [INTEGER] - [2]
```

With @Enumerated(EnumType.STRING), we can safely add new enum values or change our enums’ order. However, renaming an enum value will still break the database data.

Additionally, even though this data representation is far more readable compared to the @Enumerated(EnumType.ORDINAL) option, it also consumes a lot more space than necessary. This might turn out to be a significant issue when we need to deal with a high volume of data.

3. **Using @PostLoad and @PrePersist Annotations**:

Another option we have to deal with persisting enums in a database is to use standard JPA callback methods. We can map our enums back and forth in the @PostLoad and @PrePersist events.

The idea is to have two attributes in an entity. The first one is mapped to a database value, and the second one is a @Transient field that holds a real enum value. The transient attribute is then used by the business logic code.

To better understand the concept, let’s create a new enum and use its int value in the mapping logic:

```java
public enum Priority {
    LOW(100), MEDIUM(200), HIGH(300);

    private int priority;

    private Priority(int priority) {
        this.priority = priority;
    }

    public int getPriority() {
        return priority;
    }

    public static Priority of(int priority) {
        return Stream.of(Priority.values())
          .filter(p -> p.getPriority() == priority)
          .findFirst()
          .orElseThrow(IllegalArgumentException::new);
    }
}
```

We’ve also added the Priority.of() method to make it easy to get a Priority instance based on its int value.

Now, to use it in our Article class, we need to add two attributes and implement callback methods:

```java
@Entity
public class Article {

    @Id
    private int id;

    private String title;

    @Enumerated(EnumType.ORDINAL)
    private Status status;

    @Enumerated(EnumType.STRING)
    private Type type;

    @Basic
    private int priorityValue;

    @Transient
    private Priority priority;

    @PostLoad
    void fillTransient() {
        if (priorityValue > 0) {
            this.priority = Priority.of(priorityValue);
        }
    }

    @PrePersist
    void fillPersistent() {
        if (priority != null) {
            this.priorityValue = priority.getPriority();
        }
    }
}   
    
```

Now when persisting an Article entity:

```
Article article = new Article();
article.setId(3);
article.setTitle("callback title");
article.setPriority(Priority.HIGH);
```

JPA will trigger the following SQL query:

```
insert 
into
    Article
    (priorityValue, status, title, type, id) 
values
    (?, ?, ?, ?, ?)
binding parameter [1] as [INTEGER] - [300]
binding parameter [2] as [INTEGER] - [null]
binding parameter [3] as [VARCHAR] - [callback title]
binding parameter [4] as [VARCHAR] - [null]
binding parameter [5] as [INTEGER] - [3]
````
Even though this option gives us more flexibility in choosing the database value’s representation compared to previously described solutions, it’s not ideal. It just doesn’t feel right to have two attributes representing a single enum in the entity. Additionally, if we use this type of mapping, we aren’t able to use enum’s value in JPQL queries. 

4. **Using JPA 2.1 @Converter Annotation**:

To overcome the limitations of the solutions shown above, the JPA 2.1 release introduced a new standardized API that can be used to convert an entity attribute to a database value and vice versa. All we need to do is to create a new class that implements jakarta.persistence.AttributeConverter and annotate it with @Converter. In this example, we will use JPA 3.1. The namespace was migrated from javax to the jakarta.

Let’s see a practical example.

First, we’ll create a new enum:

```java
public enum Category {
    SPORT("S"), MUSIC("M"), TECHNOLOGY("T");

    private String code;

    private Category(String code) {
        this.code = code;
    }

    public String getCode() {
        return code;
    }
}
```

We also need to add it to the Article class:

```java
@Entity
public class Article {

    @Id
    private int id;

    private String title;

    @Enumerated(EnumType.ORDINAL)
    private Status status;

    @Enumerated(EnumType.STRING)
    private Type type;

    @Basic
    private int priorityValue;

    @Transient
    private Priority priority;

    private Category category;
}
```

Now let’s create a new CategoryConverter:

```java
@Converter(autoApply = true)
public class CategoryConverter implements AttributeConverter<Category, String> {
 
    @Override
    public String convertToDatabaseColumn(Category category) {
        if (category == null) {
            return null;
        }
        return category.getCode();
    }

    @Override
    public Category convertToEntityAttribute(String code) {
        if (code == null) {
            return null;
        }

        return Stream.of(Category.values())
          .filter(c -> c.getCode().equals(code))
          .findFirst()
          .orElseThrow(IllegalArgumentException::new);
    }
}
```

We’ve set the @Converter‘s value of autoApply to true so that JPA will automatically apply the conversion logic to all mapped attributes of a Category type. Otherwise, we’d have to put the @Converter annotation directly on the entity’s field.

Let’s now persist an Article entity:

```
Article article = new Article();
article.setId(4);
article.setTitle("converted title");
article.setCategory(Category.MUSIC);
```

Then JPA will execute the following SQL statement:

```
insert 
into
    Article
    (category, priorityValue, status, title, type, id) 
values
    (?, ?, ?, ?, ?, ?)
Converted value on binding : MUSIC -> M
binding parameter [1] as [VARCHAR] - [M]
binding parameter [2] as [INTEGER] - [0]
binding parameter [3] as [INTEGER] - [null]
binding parameter [4] as [VARCHAR] - [converted title]
binding parameter [5] as [VARCHAR] - [null]
binding parameter [6] as [INTEGER] - [4]
```

As we can see, we can simply set our own rules of converting enums to a corresponding database value if we use the AttributeConverter interface. Moreover, we can safely add new enum values or change the existing ones without breaking the already persisted data.

The overall solution is simple to implement and addresses all the drawbacks of the options presented in the earlier sections.

5. **Using Enums in JPQL**:

Let’s now see how easy it is to use enums in the JPQL queries.

To find all Article entities with Category.SPORT category, we need to execute the following statement:

```java
String jpql = "select a from Article a where a.category = com.baeldung.jpa.enums.Category.SPORT";

List<Article> articles = em.createQuery(jpql, Article.class).getResultList();
```

It’s important to note that we need to use a fully qualified enum name in this case.

Of course, we’re not limited to static queries.

It’s perfectly legal to use the named parameters:

```
String jpql = "select a from Article a where a.category = :category";

TypedQuery<Article> query = em.createQuery(jpql, Article.class);
query.setParameter("category", Category.TECHNOLOGY);

List<Article> articles = query.getResultList();
```

This example presents a very convenient way to form dynamic queries.

Additionally, we don’t need to use fully qualified names.
