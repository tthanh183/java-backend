### Is Not Null

The SQL IS NOT NULL operator is used to filter data by verifying whether a particular column has a not-null values. This operator can be used with SQL statements such as SELECT, UPDATE, and DELETE.

By using the IS NOT NULL operator, we can only fetch the records that contain valid data in a particular column.

Syntax:
```sql
SELECT column1, column2, ...
FROM table_name
WHERE column_name IS NOT NULL;
```
