###  Self Join

The SQL Self Join is used to join a table to itself as if the table were two tables. To carry this out, alias of the tables should be used at least once.

Self Join is a type of inner join, which is performed in cases where the comparison between two columns of a same table is required; probably to establish a relationship between them. In other words, a table is joined with itself when it contains both Foreign Key and Primary Key in it.

```sql
SELECT column1, column2, ...
FROM table1 T1, table1 T2
WHERE condition;
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

The following SQL query retrieves the `emp_id`, `emp_name`, `emp_salary`, `address`, and `phone_number` columns from the `employees` table where the `emp_name` is the same as the `person_name`:

```sql
SELECT T1.emp_id, T1.emp_name, T1.emp_salary, T2.address, T2.phone_number
FROM employees T1, employees T2
WHERE T1.emp_name = T2.emp_name;
```
