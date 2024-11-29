### Union Operator

The SQL UNION operator is used to combine data from multiple tables by eliminating duplicate rows (if any).

To use the UNION operator on multiple tables, all these tables must be union compatible. And they are said to be union compatible if and only if they meet the following criteria
- The same number of columns selected with the same datatype.
- The columns should be in the same order.
- They need not have same number of rows.

> [!NOTE]  
> The column names in the final result set will be based on the column names selected in the first SELECT statement. If you want to use a different name for a column in the final result set, you can use an alias in the SELECT statement.

Syntax:
```sql
SELECT column1, column2, ...
FROM table1
[WHERE condition]
UNION
SELECT column1, column2, ...
FROM table2
[WHERE condition];
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

The following SQL query retrieves the `emp_id`, `emp_name`, and `emp_salary` columns from the `employees` table where the `emp_dept` is either 'HR' or 'IT':
```sql
SELECT emp_id, emp_name, emp_salary
FROM employees
WHERE emp_dept = 'HR'
UNION
SELECT emp_id, emp_name, emp_salary
FROM employees
WHERE emp_dept = 'IT';
```

The output of the above query will be:

| emp_id | emp_name      | emp_salary |
|--------|---------------|------------|
| 1      | Alice Johnson | 60000      |
| 2      | Bob Smith     | 75000      |
| 5      | Edward Black  | 90000      |
| 7      | George Gray   | 70000      |
| 9      | Ian Red       | 65000      |
| 10     | Jackie Purple | 78000      |

