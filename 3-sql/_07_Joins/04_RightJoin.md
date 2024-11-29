### Right Join

The Right Join or Right Outer Join query in SQL returns all rows from the right table, even if there are no matches in the left table. In short, a right join returns all the values from the right table, plus matched values from the left table or NULL in case of no matching join predicate.

Syntax:
```sql
SELECT column1, column2, ...
FROM table1
RIGHT JOIN table2
ON table1.column_name = table2.column_name;
```
