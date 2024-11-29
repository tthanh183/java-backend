### Not Operator

Most of the times, there is a need to use two or more conditions to filter required records from a table; but sometimes satisfying one of the conditions would be enough. There are also scenarios when you need to retrieve records that do not satisfy the conditions specified. SQL provides logical connectives for this purpose. They are listed below −

Syntax:
```sql
SELECT column1, column2, ...
FROM table_name
WHERE NOT condition;
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

The following SQL query retrieves the `emp_id`, `emp_name`, and `emp_salary` columns from the `employees` table where the `emp_dept` is not 'IT':
```sql
SELECT emp_id, emp_name, emp_salary
FROM employees
WHERE NOT emp_dept = 'IT';
```

The output of the above query will be:

| emp_id | emp_name      | emp_salary |
|--------|---------------|------------|
| 1      | Alice Johnson | 60000      |
| 3      | Charlie Brown | 80000      |
| 4      | Diana Green   | 55000      |
| 7      | George Gray   | 70000      |
| 8      | Hannah Blue   | 85000      |
| 9      | Ian Red       | 65000      |
```

