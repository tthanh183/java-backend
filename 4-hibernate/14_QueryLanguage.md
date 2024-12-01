### Query Language

Hibernate Query Language (HQL) is an object-oriented query language, similar to SQL, but instead of operating on tables and columns, HQL works with persistent objects and their properties. HQL queries are translated by Hibernate into conventional SQL queries, which in turns perform action on database.

Although you can use SQL statements directly with Hibernate using Native SQL, but I would recommend to use HQL whenever possible to avoid database portability hassles, and to take advantage of Hibernate's SQL generation and caching strategies.

Keywords like SELECT, FROM, and WHERE, etc., are not case sensitive, but properties like table and column names are case sensitive in HQL.

1. **FROM Clause**: 

You will use FROM clause if you want to load a complete persistent objects into memory. Following is the simple syntax of using FROM clause −

```java
String hql = "FROM Employee";
Query query = session.createQuery(hql);
List results = query.list();
```

If you need to fully qualify a class name in HQL, just specify the package and class name as follows −

```java
String hql = "FROM com.tutorialspoint.Employee";
Query query = session.createQuery(hql);
List results = query.list();
```
2. **AS Clause**:

The AS clause can be used to assign aliases to the classes in your HQL queries, especially when you have the long queries. For instance, our previous simple example would be the following −
    
```java
String hql = "FROM Employee AS E";
Query query = session.createQuery(hql);
List results = query.list();
```

The AS keyword is optional and you can also specify the alias directly after the class name, as follows −

```java
String hql = "FROM Employee E";
Query query = session.createQuery(hql);
List results = query.list();
```

3. **SELECT Clause**:

The SELECT clause provides more control over the result set then the from clause. If you want to obtain few properties of objects instead of the complete object, use the SELECT clause. Following is the simple syntax of using SELECT clause to get just first_name field of the Employee object −

```java
String hql = "SELECT E.firstName FROM Employee E";
Query query = session.createQuery(hql);
List results = query.list();
```

> [!NOTE]  
> It is notable here that Employee.firstName is a property of Employee object rather than a field of the EMPLOYEE table.

4. **WHERE Clause**:

If you want to narrow the specific objects that are returned from storage, you use the WHERE clause. Following is the simple syntax of using WHERE clause −

```java
String hql = "FROM Employee E WHERE E.id = 10";
Query query = session.createQuery(hql);
List results = query.list();
```

5. **ORDER BY Clause**:

To sort your HQL query's results, you will need to use the ORDER BY clause. You can order the results by any property on the objects in the result set either ascending (ASC) or descending (DESC). Following is the simple syntax of using ORDER BY clause −

```java
String hql = "FROM Employee E WHERE E.id > 10 ORDER BY E.firstName DESC";
Query query = session.createQuery(hql);
List results = query.list();
```

If you wanted to sort by more than one property, you would just add the additional properties to the end of the order by clause, separated by commas as follows −

```java
String hql = "FROM Employee E WHERE E.id > 10 ORDER BY E.firstName DESC, E.salary DESC";
Query query = session.createQuery(hql);
List results = query.list();
```

6. **GROUP BY Clause**:

This clause lets Hibernate pull information from the database and group it based on a value of an attribute and, typically, use the result to include an aggregate value. Following is the simple syntax of using GROUP BY clause

```java
String hql = "SELECT E.id, SUM(E.salary), E.firstName FROM Employee E GROUP BY E.firstName";
Query query = session.createQuery(hql);
List results = query.list();
```
7. **USING NAMED PARAMETERS**:

Hibernate supports named parameters in its HQL queries. This makes writing HQL queries that accept input from the user easy and you do not have to defend against SQL injection attacks. Following is the simple syntax of using named parameters −

```
String hql = "FROM Employee E WHERE E.id = :employee_id";
Query query = session.createQuery(hql);
query.setParameter("employee_id", 10);
List results = query.list();
```

8. **JOIN Clause**:

The JOIN clause is used to combine columns from two or more tables based on a related column between them. Following is the simple syntax of using JOIN clause −

```java
String hql = "SELECT E.firstName, E.lastName, C.city FROM Employee E JOIN E.contact C WHERE E.id = 10";
Query query = session.createQuery(hql);
List results = query.list();
```

9. **UPDATE Clause**:

Bulk updates are new to HQL with Hibernate 3, and delete work differently in Hibernate 3 than they did in Hibernate 2. The Query interface now contains a method called executeUpdate() for executing HQL UPDATE or DELETE statements.

The UPDATE clause can be used to update one or more properties of an one or more objects. Following is the simple syntax of using UPDATE clause −

```
String hql = "UPDATE Employee set salary = 1000 WHERE id = 10";
Query query = session.createQuery(hql);
int result = query.executeUpdate();
System.out.println("Rows affected: " + result);
```

10. **DELETE Clause**:

The DELETE clause can be used to delete one or more objects. Following is the simple syntax of using DELETE clause −

```
String hql = "DELETE FROM Employee WHERE id = 10";
Query query = session.createQuery(hql);
int result = query.executeUpdate();
System.out.println("Rows affected: " + result);
```

11. **INSERT Clause**:

HQL supports INSERT INTO clause only where records can be inserted from one object to another object. Following is the simple syntax of using INSERT INTO clause −

```
String hql = "INSERT INTO Employee(firstName, lastName, salary)"  + 
             "SELECT firstName, lastName, salary FROM old_employee";
Query query = session.createQuery(hql);
int result = query.executeUpdate();
System.out.println("Rows affected: " + result);
```

12. **Aggregate Functions**:

HQL supports a range of aggregate methods, similar to SQL. They work the same way in HQL as in SQL and following is the list of the available functions −

| Function | Description                            |
|----------|----------------------------------------|
| avg()    | Returns the average of a set of values |
| count()  | Returns the number of values           |
| max()    | Returns the maximum value              |
| min()    | Returns the minimum value              |
| sum()    | Returns the sum of a set of values     |
    

13. **Sub-Queries**:

HQL supports sub-queries. You can use sub-queries in the SELECT or WHERE clauses. Following is the simple syntax of using sub-queries −

```
String hql = "FROM Employee E WHERE E.salary > (SELECT AVG(E.salary) FROM Employee)";
Query query = session.createQuery(hql);
List results = query.list();
```

14. **Pagination using Query**:

There are two methods of the Query interface for pagination.

| Method Name                     | Description                                                                                         |
|---------------------------------|-----------------------------------------------------------------------------------------------------|
| setFirstResult(int firstResult) | This method takes an integer that represents the first row in your result set, starting with row 0. |
| setMaxResults(int maxResults)   | This method takes an integer that represents the maximum number of rows to retrieve.                |

