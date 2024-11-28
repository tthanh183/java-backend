### Exists Operators

The SQL EXISTS operator is used to verify whether a particular record exists in a MySQL table. While using this operator we need to specify the record (for which you have to check the existence) using a subquery.

The EXISTS operator is used in the WHERE clause of a SELECT statement to filter records based on the existence of related records in another table.

> [!NOTE]  
> The EXISTS operator is more efficient than other operators, such as IN, because it only needs to determine whether any rows are returned by the subquery, rather than actually returning the data.

Syntax:
```sql
SELECT column1, column2, ...
FROM table_name
WHERE EXISTS (subquery);
```

> [!NOTE]  
> When you use EXISTS in SQL, it operates as follows:
> - The subquery is executed first and checks whether there is at least one row in the table that satisfies the given condition.
> - If the subquery returns at least one row, EXISTS evaluates to TRUE, and the outer query (e.g., SELECT, DELETE) is executed for the corresponding records.
> - If the subquery returns no rows, EXISTS evaluates to FALSE, and the outer query does not process or select any records.

Example:

Consider the following table named `employees`, `departments`:

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

| dept_id | dept_name | location      |
|---------|-----------|---------------|
| 1       | HR        | New York      |
| 2       | IT        | San Francisco |

The following SQL query retrieves the `emp_id`, `emp_name`, and `emp_salary` columns from the `employees` table where the `emp_dept` is 'HR':

```sql
SELECT emp_name
FROM employees e
WHERE EXISTS (
    SELECT 1
    FROM departments d
    WHERE e.emp_dept = d.dept_name
);

```

The output of the above query will be:

| emp_id | emp_name      | emp_salary |
|--------|---------------|------------|
| 1      | Alice Johnson | 60000      |
| 7      | George Gray   | 70000      |
| 9      | Ian Red       | 65000      |

Explanation:

For each employee in the employees table, it checks if the department (emp_dept) of that employee exists in the departments table.
Examples:
- For Alice (emp_dept = 'HR'): Since 'HR' exists in the departments table, the subquery returns TRUE.
- For Charlie (emp_dept = 'Finance'): Since 'Finance' does not exist in the departments table, the subquery returns FALSE.  

How EXISTS works:
- If the subquery returns TRUE, EXISTS allows the current row to be included in the result set.
- If the subquery returns FALSE, the current row is excluded from the result set.