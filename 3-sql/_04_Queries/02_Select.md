### Select Query

The SQL SELECT Statement is used to fetch the data from a database table which returns this data in the form of a table. These tables are called result-sets.

In order to filter the records to be fetched, you can use the WHERE clause along with the SELECT statement. The WHERE clause is used to filter records based on a specific condition.

1. **Selecting Data from a Table:**
Syntax:
```sql
SELECT column1, column2, column3, ...
FROM table_name;
```

If you want to fetch all the columns from a table, you can use the following syntax:
```sql
SELECT * FROM table_name;
```

2. **Computing Fields:**

You can also compute fields in the SELECT statement. For example, you can concatenate two fields together, or perform arithmetic operations on fields.

Syntax:
```sql
SELECT column1, column2, column1 + column2 AS new_column
FROM table_name;
```

3. **Alias a Column in Select**

You can also use the `AS` keyword to alias a column in the SELECT statement.

Syntax:
```sql
SELECT column1 AS alias_name
FROM table_name;
```

Example:
```sql
SELECT name AS employee_name
FROM employees;
```

4. **Example:**

Let's consider a table named `employees` with the following data:

| id | name     | age | salary   |
|----|----------|-----|----------|
| 1  | John Doe | 30  | 50000.00 |
| 2  | Jane Doe | 28  | 45000.00 |
| 3  | Alice    | 25  | 40000.00 |

Now, let's fetch the name and salary of all employees whose age is greater than 25:
```sql
SELECT name, salary
FROM employees
WHERE age > 25;
```

After executing the above query, the result will look like this:

| name     | salary   |
|----------|----------|
| John Doe | 50000.00 |
| Jane Doe | 45000.00 |

