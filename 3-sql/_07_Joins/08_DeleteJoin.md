### DELETE JOIN

The purpose of Joins in SQL is to combine records of two or more tables based on common columns/fields. Once the tables are joined, performing the deletion operation on the obtained result-set will delete records from all the original tables at a time.

Syntax:
```sql
DELETE table1, table2
FROM table1
JOIN table2
ON table1.column_name = table2.column_name
WHERE condition;
```
