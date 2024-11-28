### In Operators

The SQL IN Operator is used to specify multiple values or sub query in the WHERE clause. It returns all rows in which the specified column matches one of the values in the list. The list of values or sub query must be specified in the parenthesis e.g. IN (select query) or IN (Value1, Value2, Value3, ...).

> [!NOTE]  
> The IN operator is useful when you want to select all rows that match one of a specific set of values. While the OR operator is useful when you want to select all rows that match any one of multiple conditions.

Syntax:
```sql
SELECT column1, column2, ...
FROM table_name
WHERE column_name IN (value1, value2, ...);
```

Example1 : Using IN operator with list of values:

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
WHERE emp_dept IN ('HR', 'IT');
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

Example2 : Using IN operator with sub query:

> [!NOTE]  
> This is the SELECT statement that has a result set to be tested against the expression. The IN condition evaluates to true if any of these values match the expression.



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
WHERE emp_dept IN (SELECT emp_dept FROM employees WHERE emp_salary > 70000);
```

The output of the above query will be:

| emp_id | emp_name      | emp_salary |
|--------|---------------|------------|
| 3      | Charlie Brown | 80000      |
| 5      | Edward Black  | 90000      |
| 6      | Fiona White   | 95000      |
| 8      | Hannah Blue   | 85000      |
| 10     | Jackie Purple | 78000      |

