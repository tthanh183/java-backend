### Parameters Usage

1. **Introduction**

Building queries using JPA isn’t difficult; however, we sometimes forget simple things that make a huge difference.

One of these things is JPA query parameters, and that’s what we’ll focus on in this tutorial.

2. **What Are Query Parameters?**

Let’s start by explaining what query parameters are.

Query parameters are a way to build and execute parameterized queries. So, instead of:

```
SELECT * FROM employees e WHERE e.emp_number = '123';
```

We’d do:

```
SELECT * FROM employees e WHERE e.emp_number = ?;
```

By using a JDBC prepared statement, we need to set the parameter before executing the query:

```
pStatement.setString(1, 123);
```

3. **Why Should We Use Query Parameters?**

Instead of using query parameters, we could’ve used literals, although that’s not the recommended way to do it, as we’ll see now.

Let’s rewrite the previous query to get employees by emp_number using the JPA API, but instead of using a parameter, we’ll use a literal so we can clearly illustrate the situation:

```java
String empNumber = "A123";
TypedQuery<Employee> query = em.createQuery(
  "SELECT e FROM Employee e WHERE e.empNumber = '" + empNumber + "'", Employee.class);
Employee employee = query.getSingleResult();
```

This approach has some drawbacks:

- Embedding parameters introduce a security risk, making us vulnerable to JPQL injection attacks. Instead of the expected value, an attacker may inject any unexpected and possibly dangerous JPQL expression.
- Depending on the JPA implementation we use, and the heuristics of our application, the query cache may get exhausted. A new query may get built, compiled, and cached each time we use it with each new value/parameter. At a minimum, it won’t be efficient, and it may also lead to an unexpected OutOfMemoryError.

4. **JPA Query Parameters**

Similar to JDBC prepared statement parameters, JPA specifies two different ways to write parameterized queries by using:
- Positional parameters
- Named parameters

We may use either positional or named parameters, but we must not mix them within the same query.

- **Positional Parameters**:

Using positional parameters is one way to avoid the aforementioned issues listed earlier.

Let’s see how we would write such a query with the help of positional parameters:

```java
TypedQuery<Employee> query = em.createQuery(
  "SELECT e FROM Employee e WHERE e.empNumber = ?1", Employee.class);
String empNumber = "A123";
Employee employee = query.setParameter(1, empNumber).getSingleResult();
```

As we’ve seen with the previous example, we declare these parameters within the query by typing a question mark, followed by a positive integer number. We’ll start with 1 and move forward, incrementing it by one each time.

We may use the same parameter more than once within the same query, which makes these parameters more similar to named parameters.

Parameter numbering is a very useful feature, since it improves usability, readability, and maintenance.

It’s worth mentioning that native SQL queries support positional parameter binding, as well.

- **Collection-Valued Positional Parameters**:

As previously stated, we may also use collection-valued parameters:

```java
TypedQuery<Employee> query = entityManager.createQuery(
  "SELECT e FROM Employee e WHERE e.empNumber IN (?1)" , Employee.class);
List<String> empNumbers = Arrays.asList("A123", "A124");
List<Employee> employees = query.setParameter(1, empNumbers).getResultList();
```

- **Named Parameters**:
  
Named parameters are quite similar to positional parameters; however, by using them, we make the parameters more explicit and the query becomes more readable:

```java
TypedQuery<Employee> query = em.createQuery(
  "SELECT e FROM Employee e WHERE e.empNumber = :number" , Employee.class);
String empNumber = "A123";
Employee employee = query.setParameter("number", empNumber).getSingleResult();
```

The previous sample query is the same as the first one, but we’ve used :number, a named parameter, instead of ?1.

We can see that we declared the parameter with a colon, followed by a string identifier (JPQL identifier), which is a placeholder for the actual value that we’ll set at runtime. Before executing the query, we have to set the parameter or parameters by issuing the setParameter method.

One interesting thing to remark is that TypedQuery supports method chaining, which becomes very useful when multiple parameters have to be set.

Let’s go ahead and create a variation of the previous query using two named parameters to illustrate the method chaining:

```java
TypedQuery<Employee> query = em.createQuery(
  "SELECT e FROM Employee e WHERE e.name = :name AND e.age = :empAge" , Employee.class);
String empName = "John Doe";
int empAge = 55;
List<Employee> employees = query
  .setParameter("name", empName)
  .setParameter("empAge", empAge)
  .getResultList();
```

Here we’re retrieving all employees with a given name and age. As we clearly see, and one may expect, we can build queries with multiple parameters and as many occurrences of them as required.

If for some reason we do need to use the same parameter many times within the same query, we just need to set it once by issuing the “setParameter” method. At runtime, the specified values will replace each occurrence of the parameter.

Finally, it’s worth mentioning that the Java Persistence API specification doesn’t mandate that native queries support named parameters. Even when some implementations, like Hibernate, do support it, we need to take into account that if we do use it, the query won’t be as portable.

- **Collection-Valued Named Parameters**:

For clarity, let’s also demonstrate how this works with collection-valued parameters:

```java
TypedQuery<Employee> query = entityManager.createQuery(
  "SELECT e FROM Employee e WHERE e.empNumber IN (:numbers)" , Employee.class);
List<String> empNumbers = Arrays.asList("A123", "A124");
List<Employee> employees = query.setParameter("numbers", empNumbers).getResultList();
```

As we can see, it works in a similar way to positional parameters.

5. **Criteria Query Parameters**

A JPA query may be built by using the JPA Criteria API, which Hibernate’s official documentation explains in great detail.

In this type of query, we represent parameters by using objects instead of names or indices.

Let’s build the same query again, but this time using the Criteria API to demonstrate how to handle query parameters when dealing with CriteriaQuery:

``` 
CriteriaBuilder cb = em.getCriteriaBuilder();

CriteriaQuery<Employee> cQuery = cb.createQuery(Employee.class);
Root<Employee> c = cQuery.from(Employee.class);
ParameterExpression<String> paramEmpNumber = cb.parameter(String.class);
cQuery.select(c).where(cb.equal(c.get(Employee_.empNumber), paramEmpNumber));

TypedQuery<Employee> query = em.createQuery(cQuery);
String empNumber = "A123";
query.setParameter(paramEmpNumber, empNumber);
Employee employee = query.getResultList();
```

For this type of query, the parameter’s mechanic is a little bit different since we use a parameter object, but in essence, there’s no difference.

Within the previous example, we can see the usage of the Employee_ class. We generated this class with the Hibernate metamodel generator. These components are part of the static JPA metamodel, which allows criteria queries to be built in a strongly-typed manner.


