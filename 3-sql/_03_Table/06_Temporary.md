### Temporary Table

Temporary tables are a useful concept in MySQL. They are created in the same way as normal tables, but they are dropped automatically when the session ends. This means that the temporary table is only available and accessible to the current session.

Syntax:
```sql
CREATE TEMPORARY TABLE table_name (
    column1 datatype,
    column2 datatype,
    ...
);
```
