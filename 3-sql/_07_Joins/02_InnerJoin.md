### Inner Join

The SQL Inner Join is a type of join that combines multiple tables by retrieving records that have matching values in both tables (in the common column).

It compares each row of the first table with each row of the second table, to find all pairs of rows that satisfy the join-predicate. When the join-predicate is satisfied, the column values from both tables are combined into a new table.

Syntax:
```sql
SELECT column1, column2, ...
FROM table1
INNER JOIN table2
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
    
The following SQL query retrieves the `emp_id`, `emp_name`, `emp_salary`, `address`, and `phone_number` columns from the `employees` and `persons` tables where the `emp_name` is the same as the `person_name`:
```sql
SELECT emp_id, emp_name, emp_salary, address, phone_number
FROM employees
INNER JOIN persons
ON employees.emp_name = persons.person_name;
```

The output of the above query will be:

| emp_id | emp_name      | emp_salary | address     | phone_number |
|--------|---------------|------------|-------------|--------------|
| 1      | Alice Johnson | 60000      | 123 Main St | 555-1234     |
| 2      | Bob Smith     | 75000      | 456 Elm St  | 555-4567     |
| 3      | Charlie Brown | 80000      | 789 Oak St  | 555-7890     |




