### Group By Clause

The SQL GROUP BY clause is used in conjunction with the SELECT statement to arrange identical data into groups. This clause follows the WHERE clause in a SELECT statement and precedes the ORDER BY and HAVING clauses (if they exist).

The main purpose of grouping the records of a table based on particular columns is to perform calculations on these groups. Therefore, The GROUP BY clause is typically used with aggregate functions such as SUM(), COUNT(), AVG(), MAX(), or MIN() etc.

Syntax:
```sql
SELECT column1, column2, ...
FROM table_name
GROUP BY column1, column2, ...
Having clause is optional;
ORDER BY column1, column2, ...;
```

Example: `GROUP BY` with Aggregate Functions

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

The following SQL query retrieves the `emp_dept` and the total number of employees in each department from the `employees` table:

```sql
SELECT emp_dept, COUNT(emp_id) AS total_employees
FROM employees
GROUP BY emp_dept
HAVING COUNT(emp_id) > 1
```

The output of the above query will be:

| emp_dept  | total_employees |
|-----------|-----------------|
| HR        | 3               |
| IT        | 3               |
| Finance   | 2               |
| Marketing | 2               |

