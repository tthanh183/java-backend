### Introduction

1. **What is JDBC ?**

JDBC stands for Java Database Connectivity. It provides a set of Java API for accessing the relational databases from Java program. These Java APIs enables Java programs to execute SQL statements and interact with any SQL compliant database.

JDBC provides a flexible architecture to write a database independent application that can run on different platforms and interact with different DBMS without any modification.

Pros and Cons of JDBC:

| Pros                                                                                                                             | Cons                                                                                                                                     |
|----------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------|
| Clean and simple SQL processing Good performance with large data Very good for small applications Simple syntax so easy to learn | Complex if it is used in large projects Large programming overhead No encapsulation Hard to implement MVC concept Query is DBMS specific |

2. **Why Object Relational Mapping (ORM) ?**

When we work with an object-oriented system, there is a mismatch between the object model and the relational database. RDBMSs represent data in a tabular format whereas object-oriented languages, such as Java or C# represent it as an interconnected graph of objects.

Example:
```java
class Employee {
    private int id;
    private String name;
    private String email;
    private Address address;
    // getters and setters
}
```

```sql
CREATE TABLE EMPLOYEE (
    ID INT PRIMARY KEY,
    NAME VARCHAR(50),
    EMAIL VARCHAR(50),
    ADDRESS VARCHAR(255)
);

```

First problem, what if we need to modify the design of our database after having developed a few pages or our application? Second, loading and storing objects in a relational database exposes us to the following five mismatch problems:
- Granularity: Sometimes you will have an object model, which has more classes than the number of corresponding tables in the database.
- Inheritance: RDBMSs do not define anything similar to Inheritance, which is a natural paradigm in object-oriented programming languages.
- Identity: An RDBMS defines exactly one notion of 'sameness': the primary key. Java, however, defines both object identity (a==b) and object equality (a.equals(b)).
- Associations: Object-oriented languages represent associations using object references whereas an RDBMS represents an association as a foreign key column.
- Navigation: The ways you access objects in Java and in RDBMS are fundamentally different.

The Object-Relational Mapping (ORM) is the solution to handle all the above impedance mismatches.

3. **What is ORM ?**

ORM stands for Object-Relational Mapping (ORM) is a programming technique for converting data between relational databases and object oriented programming languages such as Java, C#, etc.

ORM has the following advantages over JDBC:
- Let’s business code access objects rather than DB tables.
- Hides details of SQL queries from OO logic.
- Based on JDBC 'under the hood.'
- No need to deal with the database implementation.
- Entities based on business concepts rather than database structure.
- Transaction management and automatic key generation.
- Fast development of application.

4. **Java ORM Frameworks**

There are many ORM frameworks available in Java. Some of the popular ones are:
- Hibernate
- JPA (Java Persistence API)
- TopLink
- iBatis
- JDO (Java Data Objects)
- EJB 3.0
- JPOX
