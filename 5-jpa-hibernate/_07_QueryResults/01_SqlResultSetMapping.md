### SqlResultSetMapping

1. **Introduction**

In this guide, we’ll take a look at SqlResultSetMapping, out of the Java Persistence API (JPA).

The core functionality here involves mapping result sets from database SQL statements into Java objects.

2. **Setup**

Before we look at its usage, let’s do some setup.

- Maven Dependency

Our required Maven dependencies are Hibernate and H2 Database. Hibernate gives us the implementation of the JPA specification.  We use H2 Database for an in-memory database.

- Database

Next, we’ll create two tables as seen here:

```sql
CREATE TABLE EMPLOYEE
(id BIGINT,
 name VARCHAR(10));
```

The EMPLOYEE table stores one result Entity object. SCHEDULE_DAYS contains records linked to the EMPLOYEE table by the column employeeId:

```sql
CREATE TABLE SCHEDULE_DAYS
(id IDENTITY,
 employeeId BIGINT,
 dayOfWeek  VARCHAR(10));
```

- Entity Objects

Our Entity objects should look similar:

```java
@Entity
public class Employee {
    @Id
    private Long id;
    private String name;
}
```

Entity objects might be named differently than database tables. We can annotate the class with @Table to explicitly map them:

```java
@Entity
@Table(name = "SCHEDULE_DAYS")
public class ScheduledDay {

    @Id
    @GeneratedValue
    private Long id;
    private Long employeeId;
    private String dayOfWeek;
}
```

3. **Scalar Mapping**

Now that we have data we can start mapping query results.

- Column Result

While SqlResultSetMapping and Query annotations work on Repository classes as well, we use the annotations on an Entity class in this example.

Every SqlResultSetMapping annotation requires only one property, name. However, without one of the member types, nothing will be mapped. The member types are ColumnResult, ConstructorResult, and EntityResult.

In this case, ColumnResult maps any column to a scalar result type:

```java
@SqlResultSetMapping(
  name="FridayEmployeeResult",
  columns={@ColumnResult(name="employeeId")})
```

The ColumnResult property name identifies the column in our query:

```java
@NamedNativeQuery(
  name = "FridayEmployees",
  query = "SELECT employeeId FROM schedule_days WHERE dayOfWeek = 'FRIDAY'",
  resultSetMapping = "FridayEmployeeResult")
```

Note that the value of resultSetMapping in our NamedNativeQuery annotation is important because it matches the name property from our ResultSetMapping declaration.

As a result, the NamedNativeQuery result set is mapped as expected. Likewise, StoredProcedure API requires this association.

- ColumnResult Test

We’ll need some Hibernate-specific objects to run our code:

```java
@BeforeAll
public static void setup() {
    emFactory = Persistence.createEntityManagerFactory("java-jpa-scheduled-day");
    em = emFactory.createEntityManager();
}
```

Finally, we call the named query to run our test:

```java
@Test
public void whenNamedQuery_thenColumnResult() {
    List<Long> employeeIds = em.createNamedQuery("FridayEmployees").getResultList();
    assertEquals(2, employeeIds.size());
}
```

4. **Constructor Mapping**

Let’s take a look at when we need to map a result set to an entire object.

- Constructor Result

Similarly to our ColumnResult example, we will add the SqlResultMapping annotation on our Entity class, ScheduledDay. However, to map using a constructor, we need to create one:

```java
public ScheduledDay (
  Long id, Long employeeId, 
  Integer hourIn, Integer hourOut, 
  String dayofWeek) {
    this.id = id;
    this.employeeId = employeeId;
    this.dayOfWeek = dayofWeek;
}
```

Also, the mapping specifies the target class and columns (both required):

```java
@SqlResultSetMapping(
    name="ScheduleResult",
    classes={
      @ConstructorResult(
        targetClass=com.baeldung.sqlresultsetmapping.ScheduledDay.class,
        columns={
          @ColumnResult(name="id", type=Long.class),
          @ColumnResult(name="employeeId", type=Long.class),
          @ColumnResult(name="dayOfWeek")})})
```

The order of the ColumnResults is very important. If columns are out of order the constructor will fail to be identified. In our example, the ordering matches the table columns, so it would not be required.

```java
@NamedNativeQuery(name = "Schedules",
        query = "SELECT * FROM schedule_days WHERE employeeId = 8",
        resultSetMapping = "ScheduleResult")
```

Another unique difference for ConstructorResult is that the resulting object instantiation is “new” or “detached”.  The mapped Entity will be in the detached state when a matching primary key exists in the EntityManager otherwise it will be new.

Sometimes we may encounter runtime errors because of mismatching SQL datatypes with Java datatypes. Therefore, we can explicitly declare it with type.


- ConstructorResult Test    

Let’s test the ConstructorResult in a unit test:

```java
@Test
public void whenNamedQuery_thenConstructorResult() {
  List<ScheduledDay> scheduleDays
    = Collections.checkedList(
      em.createNamedQuery("Schedules", ScheduledDay.class).getResultList(), ScheduledDay.class);
    assertEquals(3, scheduleDays.size());
    assertTrue(scheduleDays.stream().allMatch(c -> c.getEmployeeId().longValue() == 3));
}
```

5. **Entity Mapping**

Finally, for a simple entity mapping with less code, let’s have a look at EntityResult.

- Single Entity

EntityResult requires us to specify the entity class, Employee. We use the optional fields property for more control. Combined with FieldResult, we can map aliases and fields that do not match:

```java
@SqlResultSetMapping(
  name="EmployeeResult",
  entities={
    @EntityResult(
      entityClass = com.baeldung.sqlresultsetmapping.Employee.class,
        fields={
          @FieldResult(name="id",column="employeeNumber"),
          @FieldResult(name="name", column="name")})})
```

Now our query should include the aliased column:

```java
@NamedNativeQuery(
  name="Employees",
  query="SELECT id as employeeNumber, name FROM EMPLOYEE",
  resultSetMapping = "EmployeeResult")
```

Similarly to ConstructorResult, EntityResult requires a constructor. However, a default one works here.

- Multiple Entities

Mapping multiple entities is pretty straightforward once we have mapped a single Entity:

```java
@SqlResultSetMapping(
  name = "EmployeeScheduleResults",
  entities = {
    @EntityResult(entityClass = com.baeldung.jpa.sqlresultsetmapping.Employee.class),
    @EntityResult(entityClass = com.baeldung.jpa.sqlresultsetmapping.ScheduledDay.class)
```

- EntityResult Tests

Let’s have a look at EntityResult in action:

```java
@Test
public void whenNamedQuery_thenSingleEntityResult() {
    List<Employee> employees = Collections.checkedList(
      em.createNamedQuery("Employees").getResultList(), Employee.class);
    assertEquals(3, employees.size());
    assertTrue(employees.stream().allMatch(c -> c.getClass() == Employee.class));
}
```

Since the multiple entity results join two entities, the query annotation on only one of the classes is confusing.

For that reason, we define the query in the test:

```java
@Test
public void whenNamedQuery_thenMultipleEntityResult() {
    Query query = em.createNativeQuery(
      "SELECT e.id as idEmployee, e.name, d.id as daysId, d.employeeId, d.dayOfWeek "
        + " FROM employee e, schedule_days d "
        + " WHERE e.id = d.employeeId", "EmployeeScheduleResults");
    
    List<Object[]> results = query.getResultList();
    assertEquals(4, results.size());
    assertTrue(results.get(0).length == 2);

    Employee emp = (Employee) results.get(1)[0];
    ScheduledDay day = (ScheduledDay) results.get(1)[1];

    assertTrue(day.getEmployeeId() == emp.getId());
}
```