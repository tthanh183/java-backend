### Insert Query

The SQL INSERT INTO Statement is used to add new rows of data into a table in the database. Almost all the RDBMS provide this SQL query to add the records in database tables.

1. **Inserting Data into a Table:**

Syntax:
```sql
INSERT INTO table_name (column1, column2, column3, ...) 
VALUES (value1, value2, value3, ...),
         (value1, value2, value3, ...),
         (value1, value2, value3, ...),
         ...
```

Other Syntax:
```sql
INSERT INTO table_name VALUES (value1, value2, value3, ...),
                                 (value1, value2, value3, ...),
                                 (value1, value2, value3, ...),
                                 ...
```

> [!NOTE]  
> Make sure the order of the values is in the same order as the columns in the table, in the second syntax.

2. **Inserting Data into a Table Using Data from Another Table:**

Sometimes, you just need to copy the data from an existing table to another table in the same database. SQL provides convenient ways to do so −

Syntax:
```sql
INSERT INTO table_name1 (column1, column2, column3, ...)
SELECT column1, column2, column3, ...
FROM table_name2
WHERE condition;
```

Example:
```sql
CREATE TABLE employees (
    id INT PRIMARY KEY,
    name VARCHAR(50),
    age INT,
    address VARCHAR(255)
);

CREATE TABLE staff (
    id INT PRIMARY KEY,
    name VARCHAR(50),
    age INT,
    address VARCHAR(255)
);

INSERT INTO staff (id, name, age, address) SELECT id, name, age, address FROM employees;
```

3. **Inserting Data into a Table Using Another Table**

If you have two tables structure exactly same, then instead of selecting specific columns you can insert the contents of one table into another using the INSERT...TABLE statement.

Syntax:
```sql
INSERT INTO table_name1
TABLE table_name2;
```

Example:
```sql
CREATE TABLE employees (
    id INT PRIMARY KEY,
    name VARCHAR(50),
    age INT,
    address VARCHAR(255)
);

CREATE TABLE staff (
    id INT PRIMARY KEY,
    name VARCHAR(50),
    age INT,
    address VARCHAR(255)
);

INSERT INTO staff TABLE employees;
```
