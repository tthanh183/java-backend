### Connection 

A `Connection` in Java is a session between the Java application and the database. The `Connection` object is used to create Statement, PreparedStatement, and DatabaseMetaData. The `Connection` interface provides several methods for managing transactions, such as `commit()`, `rollback()`, and more.

By default, the connection performs a commit to apply changes to the database after executing queries.

Some important methods of the `Connection` interface are:  

| Method             | Description                                                                                  |
|--------------------|----------------------------------------------------------------------------------------------|
| close()            | Closes the connection to the database.                                                       |
| commit()           | Commits the transaction.                                                                     |
| rollback()         | Rolls back the transaction.                                                                  |
| createStatement()  | Creates a Statement object for sending SQL statements to the database.                       |
| prepareStatement() | Creates a PreparedStatement object for sending parameterized SQL statements to the database. |
| setAutoCommit()    | Sets the auto-commit mode for the connection.                                                |
| getAutoCommit()    | Returns the current auto-commit mode for the connection.                                     |

Example:
```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

public class ConnectionExample {
    public static void main(String[] args) {
        String url = "jdbc:mysql://localhost:3306/mydatabase";
        String username = "root";
        String password = "password";

        try {
            Connection connection = DriverManager.getConnection(url, username, password);
            System.out.println("Connected to the database!");
            connection.close();
        } catch (SQLException e) {
            e.printStackTrace();    
    
        }
    }
}
```
