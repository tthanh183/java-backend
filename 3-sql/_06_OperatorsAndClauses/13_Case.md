### Case 

The SQL CASE statement is a conditional statement that helps us to make decisions based on a set of conditions. It evaluates the set of conditions and returns the respective values when a condition is satisfied.

The CASE statement works like a simplified IF-THEN-ELSE statement and allows for multiple conditions to be tested.

This starts with the keyword CASE followed by multiple conditionals statements. Each conditional statement consists of at least one pair of WHEN and THEN statements. Where WHEN specifies conditional statements and THEN specifies the actions to be taken.

Syntax:
```sql
CASE
    WHEN condition1 THEN result1
    WHEN condition2 THEN result2
    ...
    ELSE result
END;
```

Example1:

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

The following SQL query retrieves the `emp_id`, `emp_name`, and `emp_salary` columns from the `employees` table and assigns a grade based on the salary:
```sql
SELECT emp_id, emp_name, emp_salary,
    CASE
        WHEN emp_salary < 60000 THEN 'C'
        WHEN emp_salary >= 60000 AND emp_salary < 80000 THEN 'B'
        ELSE 'A'
    END AS grade

FROM employees;
```

The output of the above query will be:

| emp_id | emp_name      | emp_salary | grade |
|--------|---------------|------------|-------|
| 1      | Alice Johnson | 60000      | B     |
| 2      | Bob Smith     | 75000      | B     |
| 3      | Charlie Brown | 80000      | A     |
| 4      | Diana Green   | 55000      | C     |
    

Example2 : Using CASE statement with GROUP BY clause

The following SQL query retrieves the `emp_dept` and the total number of employees in each department. It assigns a grade based on the total number of employees in each department:
```sql
SELECT emp_dept, COUNT(emp_id) AS total_employees,
    CASE
        WHEN COUNT(emp_id) < 3 THEN 'Small'
        WHEN COUNT(emp_id) >= 3 AND COUNT(emp_id) < 6 THEN 'Medium'
        ELSE 'Large'
    END AS department_size
FROM employees
GROUP BY emp_dept;
```

The output of the above query will be:

| emp_dept  | total_employees | department_size |
|-----------|-----------------|-----------------|
| HR        | 3               | Small           |
| IT        | 3               | Small           |
| Finance   | 2               | Small           |
| Marketing | 2               | Small           |



