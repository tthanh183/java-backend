### Cloning a Table

There may be a situation when you need an exact copy of a table with the same columns, attributes, indexes, default values and so forth. Instead of spending time on creating the exact same version of an existing table, you can create a clone of the existing table.

1. *Simple Cloning*:

Simple cloning operation creates a new replica table from the existing table and copies all the records in newly created table. To break this process down, a new table is created using the CREATE TABLE statement; and the data from the existing table, as a result of SELECT statement, is copied into the new table.

Syntax:
```sql
CREATE TABLE new_table_name SELECT * FROM existing_table_name;
```

> [!NOTE]  
> Copies only the basic column definitions, including data types, NULL/NOT NULL, and default values, but does not copy indexes, constraints, or AUTO_INCREMENT.

Example:
```sql
CREATE TABLE employees_copy AS
SELECT * FROM employees;
```

Output:
```
+----+------------+-----+-------------+
| id | name       | age | address     |
+----+------------+-----+-------------+
| 1  | John Doe   | 25  | 123 Main St |
| 2  | Jane Doe   | 30  | 456 Elm St  |
| 3  | John Smith | 35  | 789 Oak St  |
```

2. **Shallow Cloning**:

Shallow cloning operation creates a new replica table from the existing table but does not copy any data records into newly created table, so only new but empty table is created.

Syntax:
```sql
CREATE TABLE new_table_name LIKE existing_table_name;
```

> [!NOTE]  
> Copies the table structure along with column attributes, indexes (e.g., PRIMARY KEY, UNIQUE), and AUTO_INCREMENT, but excludes constraints like FOREIGN KEY and CHECK.

Example:
```sql
CREATE TABLE employees_copy LIKE employees;
```

Output:
```
+----+------------+-----+-------------+
| id | name       | age | address     |
+----+------------+-----+-------------+
```

3. **Deep Cloning**:

Deep cloning operation is a combination of simple cloning and shallow cloning. It not only copies the structure of the existing table but also its data into the newly created table. Hence, the new table will have all the contents from existing table and all the attributes including indices and the AUTO_INCREMENT definitions.
Syntax:
```sql
CREATE TABLE new_table LIKE original_table;
INSERT INTO new_table SELECT * FROM original_table;
```

Example:
```sql
CREATE TABLE employees_copy LIKE employees;
INSERT INTO employees_copy SELECT * FROM employees;
```

4. **Cloning in SQL Server**:

```sql
SELECT * INTO new_table_name FROM existing_table_name;
```