### Join Types

1. **Overview**

In this tutorial, we’ll look at different join types supported by JPA.

For this purpose, we’ll use JPQL, a query language for JPA.

2. **Sample Data Model**

Let’s look at our sample data model that we’ll use in the examples.

First, we’ll create an Employee entity:

```java
@Entity
public class Employee {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private long id;

    private String name;

    private int age;

    @ManyToOne
    private Department department;

    @OneToMany(mappedBy = "employee")
    private List<Phone> phones;

    // getters and setters...
}
```

Each Employee will be assigned to only one Department:

```java
@Entity
public class Department {

    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    private long id;

    private String name;

    @OneToMany(mappedBy = "department")
    private List<Employee> employees;

    // getters and setters...
}
```

Finally, each Employee will have multiple Phones:

```java
@Entity
public class Phone {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private long id;

    private String number;

    @ManyToOne
    private Employee employee;

    // getters and setters...
}
```

3. **Inner Join**

We’ll start with inner joins. When two or more entities are inner-joined, only the records that match the join condition are collected in the result.

- Implicit Inner Join With Single-Valued Association Navigation

Inner joins can be implicit. As the name implies, the developer doesn’t specify implicit inner joins. Whenever we navigate a single-valued association, JPA automatically creates an implicit join:

```java
@Test
public void whenPathExpressionIsUsedForSingleValuedAssociation_thenCreatesImplicitInnerJoin() {
    TypedQuery<Department> query
      = entityManager.createQuery(
          "SELECT e.department FROM Employee e", Department.class);
    List<Department> resultList = query.getResultList();
    
    // Assertions...
}
```

Here, the Employee entity has a many-to-one relationship with the Department entity. If we navigate from an Employee entity to her Department, specifying e.department, we’ll be navigating a single-valued association. As a result, JPA will create an inner join. Furthermore, the join condition will be derived from mapping metadata.

- Explicit Inner Join With Single-Valued Association

Next we’ll look at explicit inner joins where we use the JOIN keyword in our JPQL query:

```java
@Test
public void whenJoinKeywordIsUsed_thenCreatesExplicitInnerJoin() {
    TypedQuery<Department> query
      = entityManager.createQuery(
          "SELECT d FROM Employee e JOIN e.department d", Department.class);
    List<Department> resultList = query.getResultList();
    
    // Assertions...
}
```

In this query, we specified a JOIN keyword and the associated Department entity in the FROM clause, whereas in the previous query they weren’t specified at all. However, other than this syntactic difference, the resulting SQL queries will be very similar.

We can also specify an optional INNER keyword:

```java
@Test
public void whenInnerJoinKeywordIsUsed_thenCreatesExplicitInnerJoin() {
    TypedQuery<Department> query
      = entityManager.createQuery(
          "SELECT d FROM Employee e INNER JOIN e.department d", Department.class);
    List<Department> resultList = query.getResultList();

    // Assertions...
}
```

So since JPA automatically creates an implicit inner join, when would we need to be explicit?

First of all, JPA only creates an implicit inner join when we specify a path expression. For example, when we want to select only the Employees that have a Department, and we don’t use a path expression like e.department, we should use the JOIN keyword in our query.

Second, when we’re explicit, it can be easier to know what is going on.

- Explicit Inner Join With Collection-Valued Associations

Another place we need to be explicit is with collection-valued associations.

If we look at our data model, the Employee has a one-to-many relationship with Phone. As in an earlier example, we can try to write a similar query:

```
SELECT e.phones FROM Employee e
```

However, this won’t quite work as we may have intended. Since the selected association, e.phones, is collection-valued, we’ll get a list of Collections instead of Phone entities:

```java
@Test
public void whenCollectionValuedAssociationIsSpecifiedInSelect_ThenReturnsCollections() {
    TypedQuery<Collection> query 
      = entityManager.createQuery(
          "SELECT e.phones FROM Employee e", Collection.class);
    List<Collection> resultList = query.getResultList();

    //Assertions
}
```

Moreover, if we want to filter Phone entities in the WHERE clause, JPA won’t allow that. This is because a path expression can’t continue from a collection-valued association. So for example, e.phones.number isn’t valid.

Instead, we should create an explicit inner join and create an alias for the Phone entity. Then we can specify the Phone entity in the SELECT or WHERE clause:

```java
@Test
public void whenCollectionValuedAssociationIsJoined_ThenCanSelect() {
    TypedQuery<Phone> query 
      = entityManager.createQuery(
          "SELECT ph FROM Employee e JOIN e.phones ph WHERE ph LIKE '1%'", Phone.class);
    List<Phone> resultList = query.getResultList();
    
    // Assertions...
}
```

4. **Outer Join**

When two or more entities are outer-joined, the records that satisfy the join condition, as well as the records in the left entity, are collected in the result:

```java
@Test
public void whenLeftKeywordIsSpecified_thenCreatesOuterJoinAndIncludesNonMatched() {
    TypedQuery<Department> query 
      = entityManager.createQuery(
          "SELECT DISTINCT d FROM Department d LEFT JOIN d.employees e", Department.class);
    List<Department> resultList = query.getResultList();

    // Assertions...
}
```

Here, the result will contain Departments that have associated Employees and also the ones that don’t have any.

This is also referred to as a left outer join. JPA doesn’t provide right joins where we also collect non-matching records from the right entity. Although, we can simulate right joins by swapping entities in the FROM clause.

5. **Joins in the WHERE Clause**

- With a Condition

We can list two entities in the FROM clause and then specify the join condition in the WHERE clause.

This can be handy, especially when database level foreign keys aren’t in place:

```java
@Test
public void whenEntitiesAreListedInFromAndMatchedInWhere_ThenCreatesJoin() {
    TypedQuery<Department> query 
      = entityManager.createQuery(
          "SELECT d FROM Employee e, Department d WHERE e.department = d", Department.class);
    List<Department> resultList = query.getResultList();
    
    // Assertions...
}
```
Here, we’re joining Employee and Department entities, but this time specifying a condition in the WHERE clause.


- Without a Condition (Cartesian Product)

Similarly, we can list two entities in the FROM clause without specifying any join condition. In this case, we’ll get a cartesian product back. This means that every record in the first entity is paired with every other record in the second entity:

```java
@Test
public void whenEntitiesAreListedInFrom_ThenCreatesCartesianProduct() {
    TypedQuery<Department> query
      = entityManager.createQuery(
          "SELECT d FROM Employee e, Department d", Department.class);
    List<Department> resultList = query.getResultList();
    
    // Assertions...
}
```

As we can guess, these kinds of queries won’t perform well.

6. **Multiple Joins**

So far, we’ve used two entities to perform joins, but this isn’t a rule. We can also join multiple entities in a single JPQL query:

```java
@Test
public void whenMultipleEntitiesAreListedWithJoin_ThenCreatesMultipleJoins() {
    TypedQuery<Phone> query
      = entityManager.createQuery(
          "SELECT ph FROM Employee e
      JOIN e.department d
      JOIN e.phones ph
      WHERE d.name IS NOT NULL", Phone.class);
    List<Phone> resultList = query.getResultList();
    
    // Assertions...
}
```

Here we’re selecting all Phones of all Employees that have a Department. Similar to other inner joins, we’re not specifying conditions since JPA extracts this information from mapping metadata.

7. **Fetch Joins**

Now let’s talk about fetch joins. Their primary usage is for fetching lazy-loaded associations eagerly for the current query.

Here we’ll eagerly load Employees association:

```java
@Test
public void whenFetchKeywordIsSpecified_ThenCreatesFetchJoin() {
    TypedQuery<Department> query 
      = entityManager.createQuery(
          "SELECT d FROM Department d JOIN FETCH d.employees", Department.class);
    List<Department> resultList = query.getResultList();
    
    // Assertions...
}
```

Although this query looks very similar to other queries, there is one difference; the Employees are eagerly loaded. This means that once we call getResultList in the test above, the Department entities will have their employees field loaded, thus saving us another trip to the database.

However, we must be aware of the memory trade-off. We may be more efficient because we only performed one query, but we also loaded all Departments and their employees into memory at once.

We can also perform the outer fetch join in a similar way to outer joins, where we collect records from the left entity that don’t match the join condition. In addition, it eagerly loads the specified association:

```java
@Test
public void whenLeftAndFetchKeywordsAreSpecified_ThenCreatesOuterFetchJoin() {
    TypedQuery<Department> query 
      = entityManager.createQuery(
          "SELECT d FROM Department d LEFT JOIN FETCH d.employees", Department.class);
    List<Department> resultList = query.getResultList();
    
    // Assertions...
}
```