### ResultSet Metadata

The ResultSetMetaData interface in Java is used to retrieve metadata from a ResultSet object.

Metadata refers to information about the data. For example, metadata of a file might include the file creation date, size, etc. Metadata of a table in a database includes information about the table, such as the table name, column names, column data types, and more.

How to get ResultSet metadata:  
The getMetaData() method of the ResultSet interface returns a ResultSetMetaData object. Syntax:
```
ResultSetMetaData getMetaData() throws SQLException;
```

Methods of the ResultSetMetaData interface:

| Method                 | Description                                                                       |
|------------------------|-----------------------------------------------------------------------------------|
| getColumnCount()       | Returns the number of columns in the ResultSet.                                   |
| getColumnName(int)     | Returns the name of the specified column in the ResultSet.                        |
| getColumnType(int)     | Returns the SQL type of the specified column in the ResultSet.                    |
| getColumnTypeName(int) | Returns the database-specific type name of the specified column in the ResultSet. |
| getColumnDisplaySize() | Returns the column display size of the specified column in the ResultSet.         |
| isNullable(int)        | Returns the nullability of the specified column in the ResultSet.                 |
| isAutoIncrement(int)   | Returns whether the specified column in the ResultSet is auto-incremented.        |
| isReadOnly(int)        | Returns whether the specified column in the ResultSet is read-only.               |
| isWritable(int)        | Returns whether the specified column in the ResultSet is writable.                |
