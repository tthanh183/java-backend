### ResultSet

The `ResultSet` object maintains a cursor that points to a row in a table. Initially, the cursor points to the first row.

By default, the `ResultSet` object can only move forward and it is not updatable. However, we can make this object capable of moving both forward and backward by passing one of the two types `TYPE_SCROLL_INSENSITIVE` or `TYPE_SCROLL_SENSITIVE` in the createStatement(int, int) method. Additionally, we can make the object updatable by using the following code:
    
```java 
Statement stmt = conn.createStatement(ResultSet.TYPE_SCROLL_SENSITIVE, ResultSet.CONCUR_UPDATABLE);
```

> [!NOTE]  
> TYPE_SCROLL_INSENSITIVE: The cursor can move both forward and backward, but the result set is not sensitive to changes made by others to the database.
> TYPE_SCROLL_SENSITIVE: The cursor can move both forward and backward, and the result set is sensitive to changes made by others to the database.
> CONCUR_UPDATABLE: The result set is updatable

Some important methods of the ResultSet interface are:

| Method            | Description                                                                  |
|-------------------|------------------------------------------------------------------------------|
| next()            | Moves the cursor to the next row in the result set.                          |
| previous()        | Moves the cursor to the previous row in the result set.                      |
| first()           | Moves the cursor to the first row in the result set.                         |
| last()            | Moves the cursor to the last row in the result set.                          |
| beforeFirst()     | Moves the cursor to a position before the first row in the result set.       |
| afterLast()       | Moves the cursor to a position after the last row in the result set.         |
| absolute(int)     | Moves the cursor to an absolute position in the result set.                  |
| relative(int)     | Moves the cursor to a relative position in the result set.                   |
| getInt(int)       | Retrieves the value of the designated column in the current row.             |
| getInt(String)    | Retrieves the value of the designated column in the current row as an int.   |
| getString(int)    | Retrieves the value of the designated column in the current row as a string. |
| getString(String) | Retrieves the value of the designated column in the current row as a string. |


