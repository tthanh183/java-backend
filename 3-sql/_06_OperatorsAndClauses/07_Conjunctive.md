### Conjunctions

The SQL `AND` returns true or 1, if both its operands evaluates to true. We can use it to combine two conditions in the `WHERE` clause of an SQL statement.

```sql
SELECT column1, column2, ...
FROM table_name
WHERE condition1 AND condition2;
```
The SQL `OR` returns true or 1, if either of its operands evaluates to true. We can use it to combine two conditions in the `WHERE` clause of an SQL statement.

```sql
SELECT column1, column2, ...
FROM table_name
WHERE condition1 OR condition2;
```

Example1: 

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
WHERE emp_dept = 'HR' OR emp_dept = 'IT';
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

Example2:

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


The following SQL query retrieves the `emp_id`, `emp_name`, and `emp_salary` columns from the `employees` table where the `emp_dept` is 'HR' and the `emp_salary` is greater than 60000:

```sql
SELECT emp_id, emp_name, emp_salary
FROM employees
WHERE emp_dept = 'HR' AND emp_salary > 60000;
```

The output of the above query will be:

| emp_id | emp_name      | emp_salary |
|--------|---------------|------------|
| 2      | Bob Smith     | 75000      |
| 7      | George Gray   | 70000      |
| 9      | Ian Red       | 65000      |
| 10     | Jackie Purple | 78000      |

