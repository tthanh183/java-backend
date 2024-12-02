### ID Generator

A generator class is used to generate an ID for the for an object, which is going to be the primary key of the database table. All generator classes implement org.hibernate.id.IdentifierGenerator interface. One can create their own generator class by implementing the above-mentioned interface and overriding the generator(SharedSessionContractImplementor sess, Object obj) method.

Check the employee.hbm.xml file snippet below:

```
<hibernate-mapping>  
   <class name="com.mypackage.Employee" table="emp">  
      <id name="id">  
         <generator class="assigned"></generator>  
      </id> 
...
</hibernate-mapping>
```

Hibernate provides many predefined generator classes. Some of the important predefined generator classes in hibernate are:

| Generator Class | Description                                                                                                                                                                                                                                                            |
|-----------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `assigned`      | This generator indicates that the application will assign the primary key value. Hibernate doesn't generate any value in this case.                                                                                                                                    |
| `identity`      | This generator uses the database's auto-increment feature to generate primary key values. It works with most databases and is suitable for simple use cases. Oracle does not support identity generator. MySQL, MS SQL Server, DB2, etc. supports this.                |
| `sequence`      | This generator uses a database sequence to generate primary key values. It provides better performance and control compared to identity in some cases.                                                                                                                 |
| `increment`     | This generator generates primary key values by incrementing a value stored in memory.                                                                                                                                                                                  |
| `hilo`          | This generator uses a high-low algorithm to generate primary key values. It combines the advantages of sequence and increment.                                                                                                                                         |
| `uuid`          | This generator generates universally unique identifiers (UUIDs) as primary key values. It is useful for distributed systems where unique IDs are required.                                                                                                             |
| `native`        | This generator delegates the primary key generation strategy to the underlying database. If identity is supported by the underlying database, it chooses identity. Otherwise, it chooses sequence or hilo. It chooses the best strategy based on the database dialect. |
| `foreign`       | This generator uses the primary key value from another associated entity as the primary key value for the current entity.                                                                                                                                              |


We can use IDENTITY generator annotation to generate the ID field. See the example below:

```java

```