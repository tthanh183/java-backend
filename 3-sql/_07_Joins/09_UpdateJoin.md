### Update Join

The UPDATE statement only modifies the data in a single table and JOINS in SQL are used to fetch the combination of rows from multiple tables, with respect to a matching field.

If we want to update data in multiple tables, we can combine multiple tables into one using JOINS and then update them using UPDATE statement. This is also known as cross-table modification.

Syntax:
```sql
UPDATE table1
JOIN table2
ON table1.column_name = table2.column_name
SET table1.column_name = table2.column_name
WHERE condition;
```

Example:

Consider the following two tables named `employees` and `persons`:


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

The following SQL query updates the `emp_salary` column in the `employees` table with the `salary` column from the `persons` table where the `emp_name` is the same as the `person_name`:

```sql
UPDATE employees
JOIN persons
ON employees.emp_name = persons.person_name
SET employees.emp_salary = employees.emp_salary + 10000
    persons.phone_number = '555-0000';
```
