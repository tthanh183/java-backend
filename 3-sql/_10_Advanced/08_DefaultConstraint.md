### Default Constraint

The SQL DEFAULT Constraint is used to specify the default value for a column of a table. We usually set default value while creating the table.

The default values are treated as the column values if no values are provided while inserting the data, ensuring that the column will always have a value. We can specify default values for multiple columns in an SQL table.

Syntax:
```sql
CREATE TABLE table_name (
    column1 datatype DEFAULT default_value,
    column2 datatype DEFAULT default_value,
    ...
);
```

Passing default as Value:
```sql
INSERT INTO table_name VALUES 
(default, default, ...);
```

Add Default Constraint on Existing Table:
```sql
ALTER TABLE table_name
ALTER COLUMN column_name SET DEFAULT default_value;
```

Drop Default Constraint:
```sql
ALTER TABLE table_name
ALTER COLUMN column_name DROP DEFAULT;
```
