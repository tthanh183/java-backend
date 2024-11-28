### Delete Query

The SQL DELETE Statement is used to delete the records from an existing table. In order to filter the records to be deleted (or, delete particular records), we need to use the WHERE clause along with the DELETE statement.

If you execute DELETE statement without a WHERE clause, it will delete all the records from the table.

Syntax:
```sql
DELETE FROM table_name
WHERE [condition];
```

Example:
```sql
CREATE TABLE employees (
    id INT PRIMARY KEY,
    name VARCHAR(50),
    age INT,
    salary DECIMAL(10, 2)
);

INSERT INTO employees (id, name, age, salary)
VALUES (1, 'John Doe', 30, 50000.00),
       (2, 'Jane Doe', 28, 45000.00),
       (3, 'Alice', 25, 40000.00);

```

The table `employees` will look like this:

| id | name     | age | salary   |
|----|----------|-----|----------|
| 1  | John Doe | 30  | 50000.00 |
| 2  | Jane Doe | 28  | 45000.00 |
| 3  | Alice    | 25  | 40000.00 |

Now, let's delete the record of Alice:

```sql
DELETE FROM employees
WHERE name = 'Alice';
```

After executing the above query, the table `employees` will look like this:

| id | name     | age | salary   |
|----|----------|-----|----------|
| 1  | John Doe | 30  | 50000.00 |
| 2  | Jane Doe | 28  | 45000.00 |
    
