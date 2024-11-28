### Order By Clause

The SQL ORDER BY clause is used to sort the data in either ascending or descending order, based on one or more columns. This clause can sort data by a single column or by multiple columns. Sorting by multiple columns can be helpful when you need to sort data hierarchically, such as sorting by state, city, and then by the person's name.

In addition to sorting records in ascending order or descending order, the ORDER BY clause can also sort the data in a database table in a preferred order.

This preferred order may not sort the records of a table in any standard order (like alphabetical or lexicographical), but they could be sorted based on external condition(s).

Syntax:
```sql
SELECT column1, column2, ...
FROM table_name
ORDER BY column1 [ASC|DESC], column2 [ASC|DESC], ...;
```

Example:

Consider the following table named `employees`:

| emp_id | emp_name | emp_salary | emp_dept |
|--------|----------|------------|----------|
| 1      | John     | 50000      | HR       |
| 2      | Alice    | 60000      | IT       |
| 3      | Bob      | 70000      | HR       |
| 4      | John     | 50000      | MAR      |
| 5      | Alice    | 60000      | IT       |

The following SQL query retrieves the `emp_id`, `emp_name`, and `emp_salary` columns from the `employees` table and sorts the result by `emp_salary` in ascending order:

```sql
SELECT emp_id, emp_name, emp_salary
FROM employees
ORDER BY emp_salary;
```

The output of the above query will be:

| emp_id | emp_name | emp_salary |
|--------|----------|------------|
| 1      | John     | 50000      |
| 4      | John     | 50000      |
| 2      | Alice    | 60000      |
| 5      | Alice    | 60000      |
| 3      | Bob      | 70000      |

** Preferred Order Example **

Consider the following table named `employees`:

| emp_id | emp_name | emp_salary | emp_dept |
|--------|----------|------------|----------|
| 1      | John     | 50000      | HR       |
| 2      | Alice    | 60000      | IT       |
| 3      | Bob      | 70000      | HR       |
| 4      | Tom      | 50000      | MAR      |
| 5      | Jerry    | 60000      | IT       |

The following SQL query retrieves the `emp_id`, `emp_name`, and `emp_salary` columns from the `employees` table and sorts the result by `emp_salary` in ascending order and `emp_dept` in descending order:

```sql
SELECT emp_id, emp_name, emp_salary
FROM employees
ORDER BY (CASE WHEN emp_dept = 'HR' THEN 1
               WHEN emp_dept = 'IT' THEN 2
               ELSE 3
          END) ASC, emp_salary ASC;
```

The output of the above query will be:

| emp_id | emp_name | emp_salary |
|--------|----------|------------|
| 1      | John     | 50000      |
| 3      | Bob      | 70000      |
| 2      | Alice    | 60000      |
| 5      | Jerry    | 60000      |
| 4      | Tom      | 50000      |

