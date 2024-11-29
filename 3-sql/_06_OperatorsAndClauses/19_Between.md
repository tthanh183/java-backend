### Between Operator

The BETWEEN operator is a logical operator in SQL, that is used to retrieve the data within a specified range. The retrieved values can be integers, characters, or dates.

You can use the BETWEEN operator to replace a combination of "greater than equal AND less than equal" conditions.

Syntax:
```sql
SELECT column1, column2, ...
FROM table_name
WHERE column_name BETWEEN value1 AND value2;
```

Example:

Consider the following table named `employees`:

| emp_id | emp_name      | emp_salary | emp_dept  |
|--------|---------------|------------|-----------|
| 1      | Alice Johnson | 60000      | HR        |
| 2      | Bob Smith     | 75000      | IT        |
| 3      | Charlie Brown | 80000      | Finance   |
| 4      | Diana Green   | 55000      | Marketing |
| 5      | Edward Black  | 90000      | IT        |
| 6      | Fiona White   | 95000      | Finance   |
| 7      | George Gray   | 70000      | HR        |
| 8      | Hannah Blue   | 85000      | Marketing |
| 9      | Ian Red       | 65000      | HR        |
| 10     | Jackie Purple | 78000      | IT        |

The following SQL query retrieves the `emp_id`, `emp_name`, and `emp_salary` columns from the `employees` table where the `emp_salary` is between 70000 and 80000:

```sql
SELECT emp_id, emp_name, emp_salary
FROM employees
WHERE emp_salary BETWEEN 70000 AND 80000;
```

The output of the above query will be:

| emp_id | emp_name      | emp_salary |
|--------|---------------|------------|
| 2      | Bob Smith     | 75000      |
| 3      | Charlie Brown | 80000      |
| 7      | George Gray   | 70000      |
| 10     | Jackie Purple | 78000      |
