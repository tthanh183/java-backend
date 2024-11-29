### Is Null 

The SQL IS NULL operator is used to check whether a value in a column is NULL. It returns true if the column value is NULL; otherwise false.

The NULL is a value that represents missing or unknown data, and the IS NULL operator allows us to filter for records that contain NULL values in a particular column.

Syntax:
```sql
SELECT column1, column2, ...
FROM table_name
WHERE column_name IS NULL;
```

