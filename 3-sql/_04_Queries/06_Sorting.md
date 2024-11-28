### Sorting

The SQL ORDER BY clause is used to sort the data in ascending or descending order, based on one or more columns. By default, some databases sort the query results in an ascending order.

In addition to that, ORDER BY clause can also sort the data in a database table in a preferred order. This case may not sort the records of a table in any standard order (like alphabetical or lexicographical), but, they could be sorted based on any external condition. For instance, in an ORDERS table containing the list of orders made by various customers of an organization, the details of orders placed can be sorted based on the dates on which those orders are made. This need not be alphabetically sorted, instead, it is based on "first come first serve".

Syntax:
```sql
SELECT column1, column2, ...
FROM table_name
WHERE [condition]
ORDER BY column1, column2, ... ASC|DESC;
```

Example:

Consider the following table named `employees`:

| emp_id | emp_name | emp_salary | emp_dept |
|--------|----------|------------|----------|
| 1      | John     | 50000      | HR       |
| 2      | Alice    | 60000      | IT       |
| 3      | Bob      | 70000      | HR       |

The following SQL query sorts the records in the `employees` table based on the `emp_salary` column in ascending order:

```sql
SELECT * FROM employees
ORDER BY emp_salary DESC ;
```

The output of the above query will be:

| emp_id | emp_name | emp_salary | emp_dept |
|--------|----------|------------|----------|
| 3      | Bob      | 70000      | HR       |
| 2      | Alice    | 60000      | IT       |
| 1      | John     | 50000      | HR       |

