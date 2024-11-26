### DatabaseMetaData

The DatabaseMetaData interface in Java provides methods to retrieve metadata about the database, such as the database product name, database product version, driver name, the total number of tables, the total number of views, and more.

How to get DatabaseMetaData?

The getMetaData() method of the Connection interface returns a DatabaseMetaData object.
Syntax:
```java
public DatabaseMetaData getMetaData() throws SQLException;
```

Methods of the DatabaseMetaData interface:

| Method                      | Description                                                                      |
|-----------------------------|----------------------------------------------------------------------------------|
| getDatabaseProductName()    | Returns the name of the database product.                                        |
| getDatabaseProductVersion() | Returns the version of the database product.                                     |
| getDriverName()             | Returns the name of the JDBC driver.                                             |
| getDriverVersion()          | Returns the version of the JDBC driver.                                          |
| getURL()                    | Returns the URL of the database.                                                 |
| getUserName()               | Returns the username used to connect to the database.                            |
| getTables()                 | Returns a ResultSet object containing information about tables in the database.  |
| getColumns()                | Returns a ResultSet object containing information about columns in a table.      |
| getPrimaryKeys()            | Returns a ResultSet object containing information about primary keys in a table. |
