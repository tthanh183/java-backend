### Create Table

1. **Create Table in SQL**  

SQL provides the `CREATE TABLE` statement to create a new table in a given database. An SQL query to create a table must define the structure of a table. The structure consists of the name of a `table` and names of `columns` in the table with each column's `data type`. Note that each table must be uniquely named in a database.

Syntax:
```sql
CREATE TABLE table_name (
    column1_name data_type1,
    column2_name data_type2,
    ...
    columnN_name data_typeN,
    PRIMARY KEY (column1_name)
);
```

- `table_name`: The name of the table that you want to create.  
- `column_name`: The name of the column in the table.
- `data_type`: The data type of the column.
- `PRIMARY KEY`: The `PRIMARY KEY` constraint is used to uniquely identify each record in a table. The primary key must contain unique values, and it cannot contain NULL values.

Example:
```sql
CREATE TABLE employees (
    id INT PRIMARY KEY,
    name VARCHAR(50),
    age INT,
    address VARCHAR(255)
);
```

Once you create a table, you can check the table structure using the `DESCRIBE` statement.
    
```sql
DESCRIBE employees;
```

2. **Create Table from Existing Table**

Instead of creating a new table every time, one can also copy an existing table and its contents including its structure, into a new table. This can be done using a combination of the CREATE TABLE statement and the SELECT statement. Since its structure is copied, the new table will have the same column definitions as the original table. Furthermore, the new table would be populated using the existing values from the old table.

Syntax:
```sql
CREATE TABLE NEW_TABLE_NAME AS
SELECT [column1, column2...columnN]
FROM EXISTING_TABLE_NAME
WHERE Condition;
```

Example:
```sql
CREATE TABLE employees_copy AS
SELECT * FROM employees;
```
