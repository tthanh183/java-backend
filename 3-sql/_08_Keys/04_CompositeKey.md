### Composite Key

An SQL Composite Key is a key that can be defined on two or more columns in a table to uniquely identify any record. It can also be described as a Primary Key created on multiple columns.

Composite Keys are necessary in scenarios where a database table does not have a single column that can uniquely identify each row from the table. In such cases, we might need to use the combination of columns to ensure that each record in the table is distinct and identifiable.

Syntax:
```sql
CREATE TABLE table_name (
    column1 datatype,
    column2 datatype,
    ...
    CONSTRAINT constraint_name PRIMARY KEY (column1, column2)                 
);
```

Create Composite Key on Existing Table:
```sql
ALTER TABLE table_name ADD CONSTRAINT constraint_name PRIMARY KEY (column1, column2);
```

Drop Composite Key:
```sql
ALTER TABLE table_name DROP CONSTRAINT constraint_name;
```