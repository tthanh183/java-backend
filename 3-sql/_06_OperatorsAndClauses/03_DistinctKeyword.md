### Distinct Keyword

The SQL DISTINCT keyword is used in conjunction with the SELECT statement to fetch unique records from a table.

We use DISTINCT keyword with the SELECT statetment when there is a need to avoid duplicate values present in any specific columns/tables. When we use DISTINCT keyword, SELECT statement returns only the unique records available in the table.

Syntax:
```sql
SELECT DISTINCT column1, column2, ...
FROM table_name;
```

Example:

Consider the following table named `employees`:

| emp_id | emp_name | emp_salary | emp_dept |
|--------|----------|------------|----------|
| 1      | John     | 50000      | HR       |
| 2      | Alice    | 60000      | IT       |
| 3      | Bob      | 70000      | HR       |
| 4      | John     | 50000      | MAR      |

The following SQL query retrieves the `emp_name` and `emp_salary` columns from the `employees` table:

```sql
SELECT DISTINCT emp_name, emp_salary
FROM employees;
```

The output of the above query will be:

| emp_name | emp_salary |
|----------|------------|
| John     | 50000      |
| Alice    | 60000      |
| Bob      | 70000      |

> [!NOTE]  
> The DISTINCT keyword can be used with COUNT, SUM, AVG, etc. functions to get the unique values.
> Example:
> ```sql
> SELECT COUNT(DISTINCT emp_dept)
> FROM employees;
> ```
> The output of the above query will be:
> 
> | COUNT(DISTINCT emp_dept) |
> |--------------------------|
> | 3                        |

> [!NOTE]  
> In SQL, when there are NULL values in the column, DISTINCT treats them as unique values and includes them in the result set.