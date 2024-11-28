### Top Clause

While we are retrieving data from an SQL table, the SQL TOP clause is used to restrict the number of rows returned by a SELECT query in SQL server. In addition, we can also use it with UPDATE and DELETE statements to limit (restrict) the resultant records.

> [!NOTE]  
> The SQL TOP clause is not supported by all SQL databases. For example, MySQL uses the LIMIT clause to restrict the number of rows returned by a SELECT query.

Syntax:
```sql
SELECT LIMIT number_of_rows column1, column2, ...
FROM table_name;
```

Example:

Consider the following table named `employees`:

| emp_id | emp_name | emp_salary | emp_dept |
|--------|----------|------------|----------|
| 1      | John     | 50000      | HR       |
| 2      | Alice    | 60000      | IT       |
| 3      | Bob      | 70000      | HR       |

The following SQL query retrieves the `emp_id`, `emp_name`, and `emp_salary` columns from the `employees` table where the `emp_salary` is greater than 60000 and limits the result to 1 row:

```sql
SELECT LIMIT 1 emp_id, emp_name, emp_salary
FROM employees
WHERE emp_salary > 60000;
```

The output of the above query will be:

| emp_id | emp_name | emp_salary |
|--------|----------|------------|
| 3      | Bob      | 70000      |
    

Limit-Offset Clause: The SQL LIMIT OFFSET clause is used to restrict the number of rows returned by a SELECT query in SQL server. The OFFSET clause is used to skip a specific number of rows before returning the result set.

Syntax:
```sql
SELECT column1, column2, ...
FROM table_name
ORDER BY column_name
LIMIT number_of_rows OFFSET number_of_rows;
```
