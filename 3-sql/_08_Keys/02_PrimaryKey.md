### Primary Key

The SQL Primary Key is a column (or combination of columns) that uniquely identifies each record in a database table. The Primary Key also speeds up data access and is used to establish a relationship between tables.

> [!NOTE]  
> Even though a table can only have one Primary Key, it can be defined on one or more fields. When a primary key is created on multiple fields of a table, it is called a Composite Key.

Syntax:
```sql
CREATE TABLE table_name (
    column1 datatype PRIMARY KEY,
    column2 datatype,
    ...
);
```

Create Primary Key on Existing Table:
```sql 
ALTER TABLE table_name ADD PRIMARY KEY (column_name);
```

Drop Primary Key:
```sql
ALTER TABLE table_name DROP PRIMARY KEY;
```
