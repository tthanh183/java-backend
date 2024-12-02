### Native SQL

You can use native SQL to express database queries if you want to utilize database-specific features such as query hints or the CONNECT keyword in Oracle. Hibernate 3.x allows you to specify handwritten SQL, including stored procedures, for all create, update, delete, and load operations.

Your application will create a native SQL query from the session with the createSQLQuery() method on the Session interface −

```java
public SQLQuery createSQLQuery(String sqlString) throws HibernateException;
```
After you pass a string containing the SQL query to the createSQLQuery() method, you can associate the SQL result with either an existing Hibernate entity, a join, or a scalar result using addEntity(), addJoin(), and addScalar() methods respectively.

> [!TIP]
> It means that you can use the createSQLQuery() method to execute a native SQL query and then use the addEntity(), addJoin(), and addScalar() methods to map the result to a Hibernate entity, a join, or a scalar result.

1. **Scalar Queries**:

The most basic SQL query is to get a list of scalars (values) from one or more tables. Following is the syntax for using native SQL for scalar values −

```
String sql = "SELECT first_name, salary FROM EMPLOYEE";
SQLQuery query = session.createSQLQuery(sql);
query.setResultTransformer(Criteria.ALIAS_TO_ENTITY_MAP);
List results = query.list();
```

2. **Entity Queries**:

The above queries were all about returning scalar values, basically returning the "raw" values from the result set. Following is the syntax to get entity objects as a whole from a native sql query via addEntity().

```
String sql = "SELECT * FROM EMPLOYEE";
SQLQuery query = session.createSQLQuery(sql);
query.addEntity(Employee.class);
List results = query.list();
```

3. **Join Queries**:

You can also use native SQL to perform join queries. Following is the syntax to perform a join query using native SQL −

```
String sql = "SELECT EMPLOYEE.*, ADDRESS.* FROM EMPLOYEE JOIN ADDRESS ON EMPLOYEE.address_id = ADDRESS.id";
SQLQuery query = session.createSQLQuery(sql);
query.addEntity("EMPLOYEE", Employee.class);
query.addJoin("ADDRESS", "EMPLOYEE.address");
List results = query.list();
```

4. **Named Parameters**:

Following is the syntax to get entity objects from a native sql query via addEntity() and using named SQL query.

```
String sql = "SELECT * FROM EMPLOYEE WHERE id = :employee_id";
SQLQuery query = session.createSQLQuery(sql);
query.addEntity(Employee.class);
query.setParameter("employee_id", 10);
List results = query.list();
```

