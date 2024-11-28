### Having Clause

The SQL `HAVING` clause is similar to the `WHERE` clause; both are used to filter rows in a table based on specified criteria. However, the `HAVING` clause is used to filter grouped rows instead of single rows. These rows are grouped together by the GROUP BY clause, so, the HAVING clause must always be followed by the GROUP BY clause.

Moreover, the `HAVING` clause can be used with aggregate functions such as` COUNT()`, ` SUM()`, `AVG()`, etc., whereas the `WHERE` clause cannot be used with them.

Syntax:
```sql
SELECT column1, column2, ...
FROM table_name
GROUP BY column1, column2, ...
HAVING condition;
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

The following SQL query retrieves the `emp_dept` and the total number of employees in each department from the `employees` table:

```sql
SELECT emp_dept, COUNT(emp_id) AS total_employees
FROM employees
GROUP BY emp_dept
HAVING COUNT(emp_id) > 2;
```

The output of the above query will be:

| emp_dept  | total_employees |
|-----------|-----------------|
| HR        | 3               |
| IT        | 3               |

