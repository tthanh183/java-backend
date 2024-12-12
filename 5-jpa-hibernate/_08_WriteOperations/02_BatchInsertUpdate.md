### Batch Insert and Update

1. **Overview**

In this tutorial, we’ll learn how we can batch insert and update entities using Hibernate/JPA.

Batching allows us to send a group of SQL statements to the database in a single network call. This way, we can optimize the network and memory usage of our application.

2. **Setup**

- Sample Data Model

Let’s look at the sample data model that we’ll use in the examples.

First, we’ll create a School entity:

```java
@Entity
public class School {

    @Id
    @GeneratedValue(strategy = GenerationType.SEQUENCE)
    private long id;

    private String name;

    @OneToMany(mappedBy = "school")
    private List<Student> students;

    // Getters and setters...
}
```

Each School will have zero or more Students:

```java
@Entity
public class Student {

    @Id
    @GeneratedValue(strategy = GenerationType.SEQUENCE)
    private long id;

    private String name;

    @ManyToOne
    private School school;

    // Getters and setters...
}
```

- Tracing SQL Queries

When running our examples, we’ll need to verify that insert/update statements are indeed sent in batches. Unfortunately, we can’t tell from Hibernate log statements whether the SQL statements are batched or not. As a result, we’ll use a data source proxy to trace Hibernate/JPA SQL statements:

```java
private static class ProxyDataSourceInterceptor implements MethodInterceptor {
    private final DataSource dataSource;
    public ProxyDataSourceInterceptor(final DataSource dataSource) {
        this.dataSource = ProxyDataSourceBuilder.create(dataSource)
            .name("Batch-Insert-Logger")
            .asJson().countQuery().logQueryToSysOut().build();
    }
    
    // Other methods...
}
```

3. **Default Behaviour**

Hibernate doesn’t enable batching by default. This means that it’ll send a separate SQL statement for each insert/update operation:

```java
@Transactional
@Test
public void whenNotConfigured_ThenSendsInsertsSeparately() {
    for (int i = 0; i < 10; i++) {
        School school = createSchool(i);
        entityManager.persist(school);
    }
    entityManager.flush();
}
```

Here we persisted 10 School entities. If we look at the query logs, we can see that Hibernate sends each insert statement separately:

```
"querySize":1, "batchSize":0, "query":["insert into school (name, id) values (?, ?)"], 
  "params":[["School1","1"]]
"querySize":1, "batchSize":0, "query":["insert into school (name, id) values (?, ?)"], 
  "params":[["School2","2"]]
"querySize":1, "batchSize":0, "query":["insert into school (name, id) values (?, ?)"], 
  "params":[["School3","3"]]
"querySize":1, "batchSize":0, "query":["insert into school (name, id) values (?, ?)"], 
  "params":[["School4","4"]]
"querySize":1, "batchSize":0, "query":["insert into school (name, id) values (?, ?)"], 
  "params":[["School5","5"]]
"querySize":1, "batchSize":0, "query":["insert into school (name, id) values (?, ?)"], 
  "params":[["School6","6"]]
"querySize":1, "batchSize":0, "query":["insert into school (name, id) values (?, ?)"], 
  "params":[["School7","7"]]
"querySize":1, "batchSize":0, "query":["insert into school (name, id) values (?, ?)"], 
  "params":[["School8","8"]]
"querySize":1, "batchSize":0, "query":["insert into school (name, id) values (?, ?)"], 
  "params":[["School9","9"]]
"querySize":1, "batchSize":0, "query":["insert into school (name, id) values (?, ?)"], 
  "params":[["School10","10"]]
```

Therefore, we should configure Hibernate to enable batching. For this purpose, we should set the hibernate.jdbc.batch_size property to a number bigger than 0.

If we’re creating the EntityManager manually, we should add hibernate.jdbc.batch_size to the Hibernate properties:

```java
public Properties hibernateProperties() {
    Properties properties = new Properties();
    properties.put("hibernate.jdbc.batch_size", "5");
    
    // Other properties...
    return properties;
}
```

If we’re using Spring Boot, we can define it as an application property:

```
spring.jpa.properties.hibernate.jdbc.batch_size=5
```

4. **Batch Insert for Single Table**

- Batch Insert Without Explicit Flush

Let’s first look at how we can use batch inserts when we’re dealing with only one entity type.

We’ll use the previous code sample, but this time batching is enabled:

```java
@Transactional
@Test
public void whenInsertingSingleTypeOfEntity_thenCreatesSingleBatch() {
    for (int i = 0; i < 10; i++) {
        School school = createSchool(i);
        entityManager.persist(school);
    }
}
```

Here we persisted 10 School entities. When we look at the logs, we can verify that Hibernate sends insert statements in batches:

```
"batch":true, "querySize":1, "batchSize":5, "query":["insert into school (name, id) values (?, ?)"], 
  "params":[["School1","1"],["School2","2"],["School3","3"],["School4","4"],["School5","5"]]
"batch":true, "querySize":1, "batchSize":5, "query":["insert into school (name, id) values (?, ?)"], 
  "params":[["School6","6"],["School7","7"],["School8","8"],["School9","9"],["School10","10"]]
```

One important thing to mention here is the memory consumption. When we persist an entity, Hibernate stores it in the persistence context. For example, if we persist 100,000 entities in one transaction, we’ll end up having 100,000 entity instances in memory, possibly causing an OutOfMemoryException.

- Batch Insert With Explicit Flush

Now we’ll look at how we can optimize memory usage during batching operations. Let’s dig deep into the persistence context’s role.

First of all, the persistence context stores newly created and modified entities in memory. Hibernate sends these changes to the database when the transaction is synchronized. This generally happens at the end of a transaction. However, calling EntityManager.flush() also triggers a transaction synchronization.

Secondly, the persistence context serves as an entity cache, also referred to as the first level cache. To clear entities in the persistence context, we can call EntityManager.clear().

So to reduce the memory load during batching, we can call EntityManager.flush() and EntityManager.clear() on our application code whenever batch size is reached:

```java
@Transactional
@Test
public void whenFlushingAfterBatch_ThenClearsMemory() {
    for (int i = 0; i < 10; i++) {
        if (i > 0 && i % BATCH_SIZE == 0) {
            entityManager.flush();
            entityManager.clear();
        }
        School school = createSchool(i);
        entityManager.persist(school);
    }
}
```

Here we’re flushing the entities in the persistence context, thus making Hibernate send queries to the database. Furthermore, by clearing the persistence context, we’re removing the School entities from memory. Batching behavior will remain the same.

5. **Batch Insert for Multiple Tables**

Now let’s see how we can configure batch inserts when dealing with multiple entity types in one transaction.

When we want to persist the entities of several types, Hibernate creates a different batch for each entity type. This is because there can be only one type of entity in a single batch.

Additionally, as Hibernate collects insert statements, it creates a new batch whenever it encounters an entity type different from the one in the current batch. This is the case even though there’s already a batch for that entity type:

```java
@Transactional
@Test
public void whenThereAreMultipleEntities_ThenCreatesNewBatch() {
    for (int i = 0; i < 10; i++) {
        if (i > 0 && i % BATCH_SIZE == 0) {
            entityManager.flush();
            entityManager.clear();
        }
        School school = createSchool(i);
        entityManager.persist(school);
        Student firstStudent = createStudent(school);
        Student secondStudent = createStudent(school);
        entityManager.persist(firstStudent);
        entityManager.persist(secondStudent);
    }
}
```

Here we’re inserting a School, assigning it two Students, and repeating this process 10 times.

In the logs, we see that Hibernate sends School insert statements in several batches of size 1, while we were expecting only 2 batches of size 5. Moreover, Student insert statements are also sent in several batches of size 2, instead of 4 batches of size 5:

```
"batch":true, "querySize":1, "batchSize":1, "query":["insert into school (name, id) values (?, ?)"], 
  "params":[["School1","1"]]
"batch":true, "querySize":1, "batchSize":2, "query":["insert into student (name, school_id, id) 
  values (?, ?, ?)"], "params":[["Student-School1","1","2"],["Student-School1","1","3"]]
"batch":true, "querySize":1, "batchSize":1, "query":["insert into school (name, id) values (?, ?)"], 
  "params":[["School2","4"]]
"batch":true, "querySize":1, "batchSize":2, "query":["insert into student (name, school_id, id) 
  values (?, ?, ?)"], "params":[["Student-School2","4","5"],["Student-School2","4","6"]]
"batch":true, "querySize":1, "batchSize":1, "query":["insert into school (name, id) values (?, ?)"], 
  "params":[["School3","7"]]
"batch":true, "querySize":1, "batchSize":2, "query":["insert into student (name, school_id, id) 
  values (?, ?, ?)"], "params":[["Student-School3","7","8"],["Student-School3","7","9"]]
Other log lines...
```

To batch all insert statements of the same entity type, we should configure the hibernate.order_inserts property.

We can configure the Hibernate property manually using EntityManagerFactory:

```
public Properties hibernateProperties() {
    Properties properties = new Properties();
    properties.put("hibernate.order_inserts", "true");
    
    // Other properties...
    return properties;
}
```

If we’re using Spring Boot, we can configure the property in application.properties:

```
spring.jpa.properties.hibernate.order_inserts=true
```

After adding this property, we’ll have 1 batch for School inserts and 2 batches for Student inserts:

```
"batch":true, "querySize":1, "batchSize":5, "query":["insert into school (name, id) values (?, ?)"], 
  "params":[["School6","16"],["School7","19"],["School8","22"],["School9","25"],["School10","28"]]
"batch":true, "querySize":1, "batchSize":5, "query":["insert into student (name, school_id, id) 
  values (?, ?, ?)"], "params":[["Student-School6","16","17"],["Student-School6","16","18"],
  ["Student-School7","19","20"],["Student-School7","19","21"],["Student-School8","22","23"]]
"batch":true, "querySize":1, "batchSize":5, "query":["insert into student (name, school_id, id) 
  values (?, ?, ?)"], "params":[["Student-School8","22","24"],["Student-School9","25","26"],
  ["Student-School9","25","27"],["Student-School10","28","29"],["Student-School10","28","30"]]
```

6. **Batch Update**

Now let’s move on to batch updates. Similar to batch inserts, we can group several update statements and send them to the database in one go.

To enable this, we’ll configure the hibernate.order_updates and hibernate.batch_versioned_data properties.

If we’re creating our EntityManagerFactory manually, we can set the properties programmatically:

```
public Properties hibernateProperties() {
    Properties properties = new Properties();
    properties.put("hibernate.order_updates", "true");
    properties.put("hibernate.batch_versioned_data", "true");
    
    // Other properties...
    return properties;
}
```

And if we’re using Spring Boot, we’ll just add them to application.properties:

```
spring.jpa.properties.hibernate.order_updates=true
spring.jpa.properties.hibernate.batch_versioned_data=true
```

After configuring these properties, Hibernate should group update statements in batches:

```java
@Transactional
@Test
public void whenUpdatingEntities_thenCreatesBatch() {
    TypedQuery<School> schoolQuery = 
      entityManager.createQuery("SELECT s from School s", School.class);
    List<School> allSchools = schoolQuery.getResultList();
    for (School school : allSchools) {
        school.setName("Updated_" + school.getName());
    }
}
```

Here we’ve updated the school entities, and Hibernate sends the SQL statements in 2 batches of size 5:

```
"batch":true, "querySize":1, "batchSize":5, "query":["update school set name=? where id=?"], 
  "params":[["Updated_School1","1"],["Updated_School2","2"],["Updated_School3","3"],
  ["Updated_School4","4"],["Updated_School5","5"]]
"batch":true, "querySize":1, "batchSize":5, "query":["update school set name=? where id=?"], 
  "params":[["Updated_School6","6"],["Updated_School7","7"],["Updated_School8","8"],
  ["Updated_School9","9"],["Updated_School10","10"]]
```

7. **@Id Generation Strategy**

When we want to use batching for inserts, we should be aware of the primary key generation strategy. If our entities use the GenerationType.IDENTITY identifier generator, Hibernate will silently disable batch inserts.

Since entities in our examples use the GenerationType.SEQUENCE identifier generator, Hibernate enables batch operations:

```java
@Id
@GeneratedValue(strategy = GenerationType.SEQUENCE)
private long id;
```
