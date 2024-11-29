### Cross Join

An SQL Cross Join is a basic type of inner join that is used to retrieve the Cartesian product (or cross product) of two individual tables. That means, this join will combine each row of the first table with each row of second table (i.e. permutations).

![Cross Join](https://www.tutorialspoint.com/sql/images/crossjoin_1.jpg)

Syntax:
```sql
SELECT column1, column2, ...
FROM table1
CROSS JOIN table2;
```

Example:

Consider the following tables named `employees` and `persons`:

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

The following SQL query retrieves the `emp_id`, `emp_name`, `emp_salary`, `address`, and `phone_number` columns from the `employees` and `persons` tables:
```sql
SELECT emp_id, emp_name, emp_salary, address, phone_number
FROM employees
CROSS JOIN persons;
```

The output of the above query will be:

| emp_id | emp_name      | emp_salary | address     | phone_number |
|--------|---------------|------------|-------------|--------------|
| 1      | Alice Johnson | 60000      | 123 Main St | 555-1234     |
| 2      | Bob Smith     | 75000      | 456 Elm St  | 555-4567     |
| 3      | Charlie Brown | 80000      | 789 Oak St  | 555-7890     |
| 4      | Diana Green   | 55000      | 123 Main St | 555-1234     |
| 5      | Edward Black  | 90000      | 456 Elm St  | 555-4567     |
| 6      | Fiona White   | 95000      | 789 Oak St  | 555-7890     |
| 7      | George Gray   | 70000      | 123 Main St | 555-1234     |
| 8      | Hannah Blue   | 85000      | 456 Elm St  | 555-4567     |
| 9      | Ian Red       | 65000      | 789 Oak St  | 555-7890     |
| 10     | Jackie Purple | 78000      | 123 Main St | 555-1234     |
| 1      | Alice Johnson | 60000      | 456 Elm St  | 555-4567     |
| 2      | Bob Smith     | 75000      | 789 Oak St  | 555-7890     |
| ...    | ...           | ...        | ...         | ...          |
```



