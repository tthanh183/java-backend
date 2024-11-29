### Check Constraint

The SQL CHECK constraint is used to add conditions on a column of a table.

Once you add the check constraint on a column, it ensures that the data entered into the column meets the specified conditions. If a particular record does not meet the conditions, the database will prevent you from inserting or updating that record.

Create Check Constraint:
```sql
CREATE TABLE table_name (
    column1 datatype CHECK (condition),
    column2 datatype,
    ...
);
```

Create Check Constraint at the Table Level:
```sql
CREATE TABLE table_name (
    column1 datatype,
    column2 datatype,
    ...
    CONSTRAINT constraint_name CHECK (condition)
);
```

Add Check Constraint on Existing Table:
```sql
ALTER TABLE table_name ADD CONSTRAINT constraint_name CHECK (condition);
```

Drop Check Constraint:
```sql
ALTER TABLE table_name DROP CONSTRAINT constraint_name;
```
