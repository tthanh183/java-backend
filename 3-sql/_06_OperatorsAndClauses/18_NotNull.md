### Not Null Constraint

The NOT NULL constraint in SQL is used to ensure that a column in a table doesn't contain NULL (empty) values, and prevent any attempts to insert or update rows with NULL values.

Usually, if we don't provide value to a particular column while inserting data into a table, by default it is considered as a NULL value. But, if we add the NOT NULL constraint on a column, it will enforce that a value must be provided for that column during the data insertion, and attempting to insert a NULL value will result in a constraint violation error.

Syntax:
```sql
CREATE TABLE table_name (
    column1 datatype NOT NULL,
    column2 datatype NOT NULL,
    ...
);
```
