### Where Clause

The SQL `WHERE` clause is used to filter the results obtained by the DML statements such as `SELECT`, `UPDATE` and `DELETE` etc. We can retrieve the data from a single table or multiple tables(after join operation) using the `WHERE` clause.

Syntax:
```sql
DML_Statement column1, column2,... columnN
FROM table_name
WHERE [condition];
```

1. **DML_Statement**: The DML statement can be `SELECT`, `UPDATE`, `DELETE` etc.

Consider the following table named `employees`:

| emp_id | emp_name | emp_salary | emp_dept |
|--------|----------|------------|----------|
| 1      | John     | 50000      | HR       |
| 2      | Alice    | 60000      | IT       |
| 3      | Bob      | 70000      | HR       |

The following SQL query retrieves the `emp_id`, `emp_name`, and `emp_salary` columns from the `employees` table where the `emp_salary` is greater than 60000:

```sql
SELECT emp_id, emp_name, emp_salary
FROM employees
WHERE emp_salary > 60000;
```

The output of the above query will be:

| emp_id | emp_name | emp_salary |
|--------|----------|------------|
| 3      | Bob      | 70000      |

> [!NOTE]  
> The UPDATE and DELETE statements can also be used with the WHERE clause the same way as the SELECT statement.

2. **Where Clause with `IN` operator**:

Using the IN operator you can specify the list of values or sub query in the where clause. If you use WHERE and IN with the SELECT statement, it allows us to retrieve the rows in a table that match any of the values in the specified list.

Syntax:
```sql
SELECT column1, column2, ...
FROM table_name
WHERE column_name IN (value1, value2, ...);
```

Example:

Consider the following table named `employees`:

| emp_id | emp_name | emp_salary | emp_dept |
|--------|----------|------------|----------|
| 1      | John     | 50000      | HR       |
| 2      | Alice    | 60000      | IT       |
| 3      | Bob      | 70000      | HR       |
    
The following SQL query retrieves the `emp_id`, `emp_name`, and `emp_salary` columns from the `employees` table where the `emp_dept` is either 'HR' or 'IT':

```sql
SELECT emp_id, emp_name, emp_salary
FROM employees
WHERE emp_dept IN ('HR', 'IT');
```

The output of the above query will be:

| emp_id | emp_name | emp_salary |
|--------|----------|------------|
| 1      | John     | 50000      |
| 2      | Alice    | 60000      |
| 3      | Bob      | 70000      |
    
