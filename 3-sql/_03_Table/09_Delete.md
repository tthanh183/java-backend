### Delete Data

The SQL DELETE is a command of Data Manipulation Language (DML), so it does not delete or modify the table structure but it delete only the data contained within the table. Therefore, any constraints, indexes, or triggers defined in the table will still exist after you delete data from it.

Syntax:
```sql
DELETE FROM table_name WHERE condition;
```

Example:
```sql
CREATE TABLE employees (
    id INT PRIMARY KEY,
    name VARCHAR(50),
    age INT,
    email VARCHAR(50)
);
    

INSERT INTO employees (id, name, age, email) VALUES (1, 'John Doe', 25, ' [email protected]');
INSERT INTO employees (id, name, age, email) VALUES (2, 'Jane Doe', 30, ' [email protected]');
INSERT INTO employees (id, name, age, email) VALUES (3, 'John Smith', 35, ' [email protected]');
```

The `employees` table has three records.

| id | name       | age | email           |
|----|------------|-----|-----------------|
| 1  | John Doe   | 25  |  [email protected] |
| 2  | Jane Doe   | 30  |  [email protected] |
| 3  | John Smith | 35  |  [email protected] |

```sql
DELETE FROM employees WHERE id = 2;
```

The record with `id` 2 has been deleted from the `employees` table.

| id | name       | age | email           |
|----|------------|-----|-----------------|
| 1  | John Doe   | 25  |  [email protected] |
| 3  | John Smith | 35  |  [email protected] |
```

> [!NOTE]  
> If you omit the `WHERE` clause, all records in the table will be deleted.