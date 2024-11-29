### Left Join (or Left Outer Join)

Outer Join is used to join multiple database tables into a combined result-set, that includes all the records, even if they don't satisfy the join condition. NULL values are displayed against these records where the join condition is not met.

This scenario only occurs if the left table (or the first table) has more records than the right table (or the second table), or vice versa.

Left Join or Left Outer Join in SQL combines two or more tables, where the first table is returned wholly; but, only the matching record(s) are retrieved from the consequent tables. If zero (0) records are matched in the consequent tables, the join will still return a row in the result, but with NULL in each column from the right table.

Syntax:
```sql
SELECT column1, column2, ...
FROM table1
LEFT JOIN table2
ON table1.column_name = table2.column_name;
```

Example:

Consider the following tables named `employees` and `persons`:

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

| person_id | person_name   | address     | phone_number |
|-----------|---------------|-------------|--------------|
| 1         | Alice Johnson | 123 Main St | 555-1234     |
| 2         | Bob Smith     | 456 Elm St  | 555-4567     |
| 3         | Charlie Brown | 789 Oak St  | 555-7890     |

The following SQL query retrieves the `emp_id`, `emp_name`, `emp_salary`, `address`, and `phone_number` columns from the `employees` and `persons` tables:
```sql

SELECT emp_id, emp_name, emp_salary, address, phone_number
FROM employees
LEFT JOIN persons
ON employees.emp_name = persons.person_name;
```

The output of the above query will be:

| emp_id | emp_name      | emp_salary | address     | phone_number |
|--------|---------------|------------|-------------|--------------|
| 1      | Alice Johnson | 60000      | 123 Main St | 555-1234     |
| 2      | Bob Smith     | 75000      | 456 Elm St  | 555-4567     |
| 3      | Charlie Brown | 80000      | 789 Oak St  | 555-7890     |
| 4      | Diana Green   | 55000      | NULL        | NULL         |
| 5      | Edward Black  | 90000      | NULL        | NULL         |
| 6      | Fiona White   | 95000      | NULL        | NULL         |
| 7      | George Gray   | 70000      | NULL        | NULL         |
| 8      | Hannah Blue   | 85000      | NULL        | NULL         |
| 9      | Ian Red       | 65000      | NULL        | NULL         |
| 10     | Jackie Purple | 78000      | NULL        | NULL         |


| person_id | person_name   | address     | phone_number |
|-----------|---------------|-------------|--------------|
| 1         | Alice Johnson | 123 Main St | 555-1234     |
| 2         | Bob Smith     | 456 Elm St  | 555-4567     |
| 3         | Charlie Brown | 789 Oak St  | 555-7890     |

The above query retrieves the `emp_id`, `emp_name`, `emp_salary`, `address`, and `phone_number` columns from the `employees` and `persons` tables where the `emp_name` is the same as the `person_name`.

```sql
SELECT emp_id, emp_name, emp_salary, address, phone_number
FROM employees
LEFT JOIN persons
ON employees.emp_name = persons.person_name;
```

The output of the above query will be:

| emp_id | emp_name      | emp_salary | address     | phone_number |
|--------|---------------|------------|-------------|--------------|
| 1      | Alice Johnson | 60000      | 123 Main St | 555-1234     |
| 2      | Bob Smith     | 75000      | 456 Elm St  | 555-4567     |
| 3      | Charlie Brown | 80000      | 789 Oak St  | 555-7890     |
| 4      | Diana Green   | 55000      | NULL        | NULL         |
| 5      | Edward Black  | 90000      | NULL        | NULL         |
| 6      | Fiona White   | 95000      | NULL        | NULL         |
| 7      | George Gray   | 70000      | NULL        | NULL         |
| 8      | Hannah Blue   | 85000      | NULL        | NULL         |
| 9      | Ian Red       | 65000      | NULL        | NULL         |
| 10     | Jackie Purple | 78000      | NULL        | NULL         |
```
