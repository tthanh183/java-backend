### Not Equals

The SQL NOT EQUAL operator is used to compare two values and return true if they are not equal. It is represented by "<>" and "!=". The difference between these two is that <> follows the ISO standard, but != doesn't. So, it is recommended to use the <> operator.

Syntax:
```sql
SELECT column1, column2, ...
FROM table_name
WHERE column_name <> value;
```
