### Configuration 

Hibernate requires to know in advance — where to find the mapping information that defines how your Java classes relate to the database tables. Hibernate also requires a set of configuration settings related to database and other related parameters. All such information is usually supplied as a standard Java properties file called hibernate.properties, or as an XML file named hibernate.cfg.xml.

I will consider XML formatted file hibernate.cfg.xml to specify required Hibernate properties in my examples. Most of the properties take their default values and it is not required to specify them in the property file unless it is really required. This file is kept in the root directory of your application's classpath.

1. **Hibernate Properties**

| Property Name                     | Description                                                                                       |
|-----------------------------------|---------------------------------------------------------------------------------------------------|
| hibernate.dialect                 | This property makes Hibernate generate the appropriate SQL statements for the chosen database.    |
| hibernate.connection.driver_class | The JDBC driver class.                                                                            |
| hibernate.connection.url          | The JDBC URL to your database.                                                                    |
| hibernate.connection.username     | The database username.                                                                            |
| hibernate.connection.password     | The database password.                                                                            |
| hibernate.show_sql                | Outputs the SQL statements to the console.                                                        |
| hibernate.format_sql              | Pretty print the SQL in the console.                                                              |
| hibernate.connection.pool_size    | The maximum number of connections in the connection pool.                                         |
| hibernate.connection.autocommit   | This property is used to enable/disable the autocommit mode.                                      |
| hibernate.hbm2ddl.auto            | Automatically validates or exports schema DDL to the database when the SessionFactory is created. |

Example of hibernate.cfg.xml file with MySQL database:

```
<?xml version='1.0' encoding='UTF-8'?>  
<!DOCTYPE hibernate-configuration PUBLIC  
   "-//Hibernate/Hibernate Configuration DTD 5.3//EN"  
   "http://hibernate.org/dtd/hibernate-configuration-3.0.dtd">  

<hibernate-configuration>  
   <session-factory>  
      <property name="hbm2ddl.auto">update</property>  
      <property name="dialect">org.hibernate.dialect.MySQL8Dialect</property>  
      <property name="connection.url">jdbc:mysql://localhost/TUTORIALSPOINT</property>  
      <property name="connection.username">root</property>  
      <property name="connection.password">guest123</property>  
      <property name="connection.driver_class">com.mysql.cj.jdbc.Driver</property>  
      <mapping resource="employee.hbm.xml"/>  
   </session-factory>  
</hibernate-configuration>
```

> [!NOTE]  
> hbm2ddl.auto Property
> - create: Hibernate first drops the existing schema and then creates a new schema.
> - update: Hibernate first checks the existing schema, then updates it. If not, it creates a new schema.
> - validate: Hibernate only validates the schema.
> - create-drop: Hibernate creates the schema automatically when the SessionFactory is created and drops the schema when the SessionFactory is closed explicitly.
> - none: This value disables the automatic DDL generation.

