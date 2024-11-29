### Except Operator

The EXCEPT operator in SQL is used to retrieve all the unique records from the left operand (query), except the records that are present in the result set of the right operand (query).

In other words, this operator compares the distinct values of the left query with the result set of the right query. If a value from the left query is found in the result set of the right query, it is excluded from the final result.

Syntax:
```sql
SELECT column1, column2, ...
FROM table1
EXCEPT
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

| person_id | person_name   | address      | phone_number |
|-----------|---------------|--------------|--------------|
| 1         | Alice Johnson | 123 Main St  | 555-1234     |
| 2         | Bob Smith     | 456 Elm St   | 555-4567     |
| 3         | Charlie Brown | 789 Oak St   | 555-7890     |
| 4         | Diana Green   | 101 Pine St  | 555-1011     |
| 5         | Edward Black  | 202 Cedar St | 555-2022     |
| 6         | Fiona White   | 303 Elm St   | 555-3033     |

The following SQL query retrieves the `emp_id`, `emp_name` columns from the `employees` table that are not common with the `person_id`, `person_name` columns from the `persons` table:
```sql
SELECT emp_id, emp_name
FROM employees
EXCEPT
SELECT person_id, person_name
FROM persons;
```

The output of the above query will be:

| emp_id | emp_name      |
|--------|---------------|
| 7      | George Gray   |
| 8      | Hannah Blue   |
| 9      | Ian Red       |
| 10     | Jackie Purple |
```