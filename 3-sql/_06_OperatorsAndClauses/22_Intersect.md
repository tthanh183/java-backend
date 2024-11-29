### Intersect Operator

In mathematical set theory, the intersection of two sets is a collection of values that are common to both sets.

The INTERSECT operator in SQL is used to retrieve the records that are identical/common between the result sets of two or more tables.

Syntax:
```sql
SELECT column1, column2, ...
FROM table1
INTERSECT
SELECT column1, column2, ...
FROM table2;
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

| person_id | person_name   | address     | phone_number |
|-----------|---------------|-------------|--------------|
| 1         | Alice Johnson | 123 Main St | 555-1234     |
| 2         | Bob Smith     | 456 Elm St  | 555-4567     |
| 3         | Charlie Brown | 789 Oak St  | 555-7890     |

The following SQL query retrieves the `emp_id`, `emp_name` columns from the `employees` table that are common with the `person_id`, `person_name` columns from the `persons` table:
```sql
SELECT emp_id, emp_name
FROM employees
INTERSECT
SELECT person_id, person_name
FROM persons;
```

The output of the above query will be:

| emp_id | emp_name      |
|--------|---------------|
| 1      | Alice Johnson |
| 2      | Bob Smith     |
| 3      | Charlie Brown |
```


