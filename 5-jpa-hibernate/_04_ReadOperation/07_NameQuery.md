### Named Query 

1. **Overview** 

A major disadvantage of having HQL and SQL scattered across data access objects is that it makes the code unreadable. Hence, it might make sense to group all HQL and SQL in one place and use only their reference in the actual data access code. Fortunately, Hibernate allows us to do this with named queries.

A named query is a statically defined query with a predefined unchangeable query string. They’re validated when the session factory is created, thus making the application to fail fast in case of an error.

In this article, we’ll see how to define and use Hibernate Named Queries using the @NamedQuery and @NamedNativeQuery annotations.

2. **The Entity**

Let’s first look at the entity we’ll be using in this article:

```java
@Entity
public class DeptEmployee {
    @Id
    @GeneratedValue(strategy = GenerationType.SEQUENCE)
    private long id;

    private String employeeNumber;

    private String designation;

    private String name;

    @ManyToOne
    private Department department;

    // getters and setters
}
```

In our example, we’ll retrieve an employee based on their employee number.

3. **Named Query**

To define this as a named query, we’ll use the org.hibernate.annotations.NamedQuery annotation. It extends the javax.persistence.NamedQuery with Hibernate features.

We’ll define it as an annotation of the DeptEmployee class:

```
@org.hibernate.annotations.NamedQuery(name = "DeptEmployee_findByEmployeeNumber", 
  query = "from DeptEmployee where employeeNumber = :employeeNo")
```

It’s important to note that every @NamedQuery annotation is attached to exactly one entity class or mapped superclass. But, since the scope of named queries is the entire persistence unit, we should select the query name carefully to avoid a collision. And we have achieved this by using the entity name as a prefix.

If we have more than one named query for an entity, we’ll use the @NamedQueries annotation to group these:

```org.hibernate.annotations.NamedQueries({
    @org.hibernate.annotations.NamedQuery(name = "DeptEmployee_FindByEmployeeNumber", 
      query = "from DeptEmployee where employeeNumber = :employeeNo"),
    @org.hibernate.annotations.NamedQuery(name = "DeptEmployee_FindAllByDesgination", 
      query = "from DeptEmployee where designation = :designation"),
    @org.hibernate.annotations.NamedQuery(name = "DeptEmployee_UpdateEmployeeDepartment", 
      query = "Update DeptEmployee set department = :newDepartment where employeeNumber = :employeeNo"),
...
})
```

Note that the HQL query can be a DML-style operation. So, it doesn’t need to be a select statement only. For example, we can have an update query as in DeptEmployee_UpdateEmployeeDesignation above.

- Configuring Query Features

We can set various query features with the @NamedQuery annotation. Let’s look at an example:

```
@org.hibernate.annotations.NamedQuery(
  name = "DeptEmployee_FindAllByDepartment", 
  query = "from DeptEmployee where department = :department",
  timeout = 1,
  fetchSize = 10
)
```

Here, we’ve configured the timeout interval and the fetch size. Apart from these two, we can also set features such as:
    - cacheable – whether the query (results) is cacheable or not
    - cacheMode – the cache mode used for this query; this can be one of GET, IGNORE, NORMAL, PUT, or REFRESH
    - cacheRegion – if the query results are cacheable, name the query cache region to use
    - comment – a comment added to the generated SQL query; targetted for DBAs
    - flushMode – the flush mode for this query, one of ALWAYS, AUTO, COMMIT, MANUAL, or PERSISTENCE_CONTEXT

- Using the Named Query

Now that we’ve defined the named query, let’s use it to retrieve an employee:

```
Query<DeptEmployee> query = session.createNamedQuery("DeptEmployee_FindByEmployeeNumber", 
  DeptEmployee.class);
query.setParameter("employeeNo", "001");
DeptEmployee result = query.getSingleResult();
```

Here, we’ve used the createNamedQuery method. It takes the name of the query and returns an org.hibernate.query.Query object.

4. **Named Native Query**

As well as HQL queries, we can also define native SQL as a named query. To do this, we can use the @NamedNativeQuery annotation. Though it is similar to the @NamedQuery, it requires a bit more configuration.

Let’s explore this annotation using an example:

```java
@org.hibernate.annotations.NamedNativeQueries(
    @org.hibernate.annotations.NamedNativeQuery(name = "DeptEmployee_FindByEmployeeName", 
      query = "select * from deptemployee emp where name=:name",
      resultClass = DeptEmployee.class)
)
```

Since this is a native query, we’ll have to tell Hibernate what entity class to map the results to. Consequently, we’ve used the resultClass property for doing this.

Another way to map the results is to use the resultSetMapping property. Here, we can specify the name of a pre-defined SQLResultSetMapping.

Note that we can use only one of resultClass and resultSetMapping.






