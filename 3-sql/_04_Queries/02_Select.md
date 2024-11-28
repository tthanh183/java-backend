### Select Query

The SQL SELECT Statement is used to fetch the data from a database table which returns this data in the form of a table. These tables are called result-sets.

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
