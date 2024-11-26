### PreparedStatement 

The PreparedStatement interface in Java is a sub-interface of Statement. It is used to execute parameterized queries.

Example:
```
String sql = "INSERT INTO student VALUES(?, ?, ?)";
```
As you can see, we use placeholders (?) for the values. These placeholders will be set with actual values by calling the setter methods of the PreparedStatement.

Why use PreparedStatement?  
The performance of the application is better when using the PreparedStatement interface because the query is compiled only once.

How to create a PreparedStatement object?  
The prepareStatement() method of the Connection interface is used to return a PreparedStatement object. Syntax:
```
PreparedStatement preparedStatement = connection.prepareStatement(sql);
```

Methods of the PreparedStatement interface:

| Method          | Description                                                                                     |
|-----------------|-------------------------------------------------------------------------------------------------|
| setInt()        | Sets the designated parameter to the given Java int value.                                      |
| setString()     | Sets the designated parameter to the given Java String value.                                   |
| setDouble()     | Sets the designated parameter to the given Java double value.                                   |
| setFloat()      | Sets the designated parameter to the given Java float value.                                    |
| setDate()       | Sets the designated parameter to the given Java Date value.                                     |
| executeUpdate() | Executes the SQL query in the PreparedStatement object and returns the number of affected rows. |
| executeQuery()  | Executes the SQL query in the PreparedStatement object and returns a ResultSet object.          |
| close()         | Closes the PreparedStatement object.                                                            |