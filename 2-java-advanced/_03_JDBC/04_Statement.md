### Statement

The `Statement` interface in Java provides methods for executing SQL queries against a database. The `Statement` interface is a factory for `ResultSet`, meaning it provides methods to create `ResultSet` objects.

Some important methods of the `Statement` interface are:

| Method          | Description                                                                                     |
|-----------------|-------------------------------------------------------------------------------------------------|
| executeQuery()  | Executes a SELECT query and returns a ResultSet object containing the query's results.          |
| executeUpdate() | Executes an INSERT, UPDATE, or DELETE query and returns the number of affected rows.            |
| execute()       | Executes any SQL query and returns a boolean value indicating the query type (true for SELECT). |
| close()         | Closes the Statement object.                                                                    |
    
Example:
```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.Statement;

public class StatementExample {
    public static void main(String[] args) {
        String url = "jdbc:mysql://localhost:3306/mydatabase";
        String username = "root";
        String password = "password";

        try {
            Connection connection = DriverManager.getConnection(url, username, password);
            Statement statement = connection.createStatement();
            String query = "SELECT * FROM users";
            statement.executeQuery(query);
            System.out.println("Query executed successfully!");
            statement.close();
            connection.close();
        } catch (SQLException e) {
            e.printStackTrace();
        }   

    }
}
```
