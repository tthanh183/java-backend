### Update Query

The SQL UPDATE Statement is used to modify the existing records in a table. This statement is a part of Data Manipulation Language (DML), as it only modifies the data present in a table without affecting the table's structure.

To filter records that needs to be modified, you can use a WHERE clause with UPDATE statement. Using a WHERE clause, you can either update a single row or multiple rows.

Syntax:
```sql
UPDATE table_name
SET column1 = value1, column2 = value2, ...
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

Now, let's update the salary of John Doe to 55000.00:

```sql
UPDATE employees
SET salary = 55000.00
WHERE name = 'John Doe';
```

After executing the above query, the table `employees` will look like this:

| id | name     | age | salary   |
|----|----------|-----|----------|
| 1  | John Doe | 30  | 55000.00 |
| 2  | Jane Doe | 28  | 45000.00 |
| 3  | Alice    | 25  | 40000.00 |



