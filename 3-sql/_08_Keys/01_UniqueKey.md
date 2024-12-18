### Unique Key

The SQL Unique Key (or, Unique constraint) does not allow duplicate values in a column of a table. It prevents two records from having same values in a column. 

> [!NOTE]  
> The unique key accept only one NULL value. While the primary key does not accept any NULL value.

Syntax:
```sql
CREATE TABLE table_name (
    column1 datatype UNIQUE,
    column2 datatype,
    ...
);
```

Create Unique Key on Existing Table:
```sql
ALTER TABLE table_name ADD UNIQUE (column_name);
```

Drop a Unique Key:
```sql
ALTER TABLE table_name DROP CONSTRAINT UNIQUE_KEY_NAME;
```