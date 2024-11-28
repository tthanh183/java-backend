### Update Views

Unlike CREATE VIEW and DROP VIEW there is no direct statement to update the records of an existing view. We can use the SQL UPDATE Statement to modify the existing records in a table or a view.

Syntax:
```sql
UPDATE view_name
SET column1 = value1, column2 = value2, ...
WHERE condition;
```

Example:

Consider the following view named `employee_view` that contains the `emp_id`, `emp_name`, and `emp_salary` columns from the `employees` table:

| emp_id | emp_name | emp_salary |
|--------|----------|------------|
| 1      | John     | 50000      |
| 2      | Alice    | 60000      |
| 3      | Bob      | 70000      |

The following SQL query updates the `emp_salary` of the employee with `emp_id` 1 in the `employee_view`:

```sql
UPDATE employee_view
SET emp_salary = 55000
WHERE emp_id = 1;
```

The output of the above query will be:

| emp_id | emp_name | emp_salary |
|--------|----------|------------|
| 1      | John     | 55000      |
| 2      | Alice    | 60000      |
| 3      | Bob      | 70000      |

