### Truncate Table

SQL provides command to `TRUNCATE` a table completely in one go instead of deleting table records one by one which will be very time consuming and cumbersome process.

The SQL `TRUNCATE` TABLE command is used to empty a table. This command is a sequence of `DROP TABLE` and `CREATE TABLE` statements and requires the `DROP` privilege.

You can also use `DROP TABLE` command to delete a table but it will remove the complete table structure from the database and you would need to re-create this table once again if you wish you store some data again.

Syntax:
```sql
TRUNCATE TABLE table_name;
```

Example:
```sql
CREATE TABLE employees (
    id INT PRIMARY KEY,
    name VARCHAR(50),
    age INT,
    address VARCHAR(255)
);

INSERT INTO employees (id, name, age, address) VALUES (1, 'John Doe', 25, '123 Main St');
INSERT INTO employees (id, name, age, address) VALUES (2, 'Jane Doe', 30, '456 Elm St');
INSERT INTO employees (id, name, age, address) VALUES (3, 'John Smith', 35, '789 Oak St');
```

The `employees` table has three records.

| id | name       | age | address     |
|----|------------|-----|-------------|
| 1  | John Doe   | 25  | 123 Main St |
| 2  | Jane Doe   | 30  | 456 Elm St  |
| 3  | John Smith | 35  | 789 Oak St  |

```sql
TRUNCATE TABLE employees;
```

The `employees` table has been truncated and now it is empty.

| id | name | age | address |
|----|------|-----|---------|

