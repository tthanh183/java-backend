### Like Operators

The SQL `LIKE` operator is used to retrieve the data in a column of a table, based on a specified pattern.

It is used along with the `WHERE` clause of the `UPDATE`, `DELETE` and `SELECT` statements, to filter the rows based on the given pattern. These patterns are specified using Wildcards.

Syntax:
```sql
SELECT column1, column2, ...
FROM table_name
WHERE column_name LIKE pattern;
```

Some of the commonly used Wildcards are:
| Wildcard | Description |
|----------|-------------|
| %        | Represents zero or more characters |
| _        | Represents a single character |

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

The following SQL query retrieves the `emp_id`, `emp_name`, and `emp_salary` columns from the `employees` table where the `emp_name` starts with 'J':
```sql

SELECT emp_id, emp_name, emp_salary
FROM employees
WHERE emp_name LIKE 'J%';
```

The output of the above query will be:

| emp_id | emp_name      | emp_salary |
|--------|---------------|------------|
| 10     | Jackie Purple | 78000      |
```

> [!NOTE]  
> Escape characters can be used to search for the actual '%' and '_' characters in the column values. The escape character is specified using the `ESCAPE` keyword.  
> Syntax:
> ```sql
> SELECT column1, column2, ...
> FROM table_name
> WHERE column_name LIKE pattern ESCAPE escape_character;
> ```




