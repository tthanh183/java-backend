### Lazy Element Collections

1. **Overview**

The JPA specification provides two different fetch strategies: eager and lazy. While the lazy approach helps to avoid unnecessarily loading data that we don’t need, we sometimes need to read data not initially loaded in a closed Persistence Context. Moreover, accessing lazy element collections in a closed Persistence Context is a common problem.

In this tutorial, we’ll focus on how to load data from lazy element collections. We’ll explore three different solutions: one involving the JPA query language, another with the use of entity graphs, and the last one with transaction propagation.

2. **The Element Collection Problem**

By default, JPA uses the lazy fetch strategy in associations of type @ElementCollection. Thus, any access to the collection in a closed Persistence Context will result in an exception.

To understand the problem, let’s define a domain model based on the relationship between the employee and its phone list:

```java
@Entity
public class Employee {
    @Id
    private int id;
    private String name;
    @ElementCollection
    @CollectionTable(name = "employee_phone", joinColumns = @JoinColumn(name = "employee_id"))
    private List phones;

    // standard constructors, getters, and setters
}

@Embeddable
public class Phone {
    private String type;
    private String areaCode;
    private String number;

    // standard constructors, getters, and setters
}
```

Our model specifies that an employee can have many phones. The phone list is a collection of embeddable types. Let’s use a Spring Repository with this model:

```java
@Repository
public class EmployeeRepository {

    public Employee findById(int id) {
        return em.find(Employee.class, id);
    }

    // additional properties and auxiliary methods
}
```

Now, let’s reproduce the problem with a simple JUnit test case:

```java
public class ElementCollectionIntegrationTest {

    @Before
    public void init() {
        Employee employee = new Employee(1, "Fred");
        employee.setPhones(
          Arrays.asList(new Phone("work", "+55", "99999-9999"), new Phone("home", "+55", "98888-8888")));
        employeeRepository.save(employee);
    }

    @After
    public void clean() {
        employeeRepository.remove(1);
    }

    @Test(expected = org.hibernate.LazyInitializationException.class)
    public void whenAccessLazyCollection_thenThrowLazyInitializationException() {
        Employee employee = employeeRepository.findById(1);
 
        assertThat(employee.getPhones().size(), is(2));
    }
}
```

This test throws an exception when we try to access the phone list because the Persistence Context is closed.

We can solve this problem by changing the fetch strategy of the @ElementCollection to use the eager approach. However, fetching the data eagerly isn’t necessarily the best solution, since the phone data always will be loaded, whether we need it or not.

3. **Loading Data with JPA Query Language**

The JPA query language allows us to customize the projected information. Therefore, we can define a new method in our EmployeeRepository to select the employee and its phones:

```java
public Employee findByJPQL(int id) {
    return em.createQuery("SELECT u FROM Employee AS u JOIN FETCH u.phones WHERE u.id=:id", Employee.class)
        .setParameter("id", id).getSingleResult();
}
```

The above query uses an inner join operation to fetch the phone list for each employee returned.

4. **Loading Data with Entity Graph**

Another possible solution is to use the entity graph feature from JPA. The entity graph makes it possible for us to choose which fields will be projected by JPA queries. Let’s define one more method in our repository:

```java
public Employee findByEntityGraph(int id) {
    EntityGraph entityGraph = em.createEntityGraph(Employee.class);
    entityGraph.addAttributeNodes("name", "phones");
    Map<String, Object> properties = new HashMap<>();
    properties.put("javax.persistence.fetchgraph", entityGraph);
    return em.find(Employee.class, id, properties);
}
```

We can see that our entity graph includes two attributes: name and phones. So, when JPA translates this to SQL, it’ll project the related columns.

5. **Loading Data in a Transactional Scope**

Finally, we’re going to explore one last solution. So far, we’ve seen that the problem is related to the Persistence Context life cycle.

What happens is that our Persistence Context is transaction-scoped and will remain open until the transaction finishes. The transaction life cycle spans from the beginning to the end of the execution of the repository method.

So, let’s create another test case and configure our Persistence Context to bind to a transaction started by our test method. We’ll keep the Persistence Context open until the test ends:

```java
@Test
@Transactional
public void whenUseTransaction_thenFetchResult() {
    Employee employee = employeeRepository.findById(1);
    assertThat(employee.getPhones().size(), is(2));
}
```

The @Transactional annotation configures a transactional proxy around the instance of the related test class. Moreover, the transaction is associated with the thread executing it. Considering the default transaction propagation setting, every Persistence Context created from this method joins to this same transaction. Consequently, the transaction persistence context is bound to the transaction scope of the test method.
