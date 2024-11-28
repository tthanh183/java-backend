### Views

A view in SQL is a virtual table that is stored in the database with an associated name. It is actually a composition of a table in the form of a predefined SQL query. A view can contain rows from an existing table (all or selected). A view can be created from one or many tables. Unless indexed, a view does not exist in a database.

The data in the view does not exist in the database physically. A view is typically created by the database administrator and is used to:
- Structure data in a way that users or classes of users find natural or intuitive.
- Restrict access to the data in such a way that a user can see and (sometimes) modify exactly what they need and no more.
- Summarize data from various tables which can be used to generate reports.

Syntax:
```sql
CREATE VIEW view_name AS
SELECT column1, column2, ...
FROM table_name
WHERE condition;
```

**WITH CHECK OPTION**

Syntax:
```sql
CREATE VIEW view_name AS
SELECT column1, column2, ...
FROM table_name
WHERE condition
WITH CHECK OPTION;
```

> [!NOTE]  
> The WITH CHECK OPTION is a CREATE VIEW statement option. The purpose of the WITH CHECK OPTION is to ensure that all UPDATE and INSERT statements satisfy the condition(s) specified by the WHERE clause.

Example:

Consider the following table named `employees`:

| emp_id | emp_name | emp_salary | emp_dept |
|--------|----------|------------|----------|
| 1      | John     | 50000      | HR       |
| 2      | Alice    | 60000      | IT       |
| 3      | Bob      | 70000      | HR       |


The following SQL query creates a view named `employee_view` that contains the `emp_id`, `emp_name`, and `emp_salary` columns from the `employees` table:

```sql
CREATE VIEW employee_view AS
SELECT emp_id, emp_name, emp_salary
FROM employees;
```

The output of the above query will be:

| emp_id | emp_name | emp_salary |
|--------|----------|------------|
| 1      | John     | 50000      |
| 2      | Alice    | 60000      |
| 3      | Bob      | 70000      |


