### Full Join (or Full Outer Join)

SQL Full Join creates a new table by joining two tables as a whole. The joined table contains all records from both the tables and fills NULL values for missing matches on either side. In short, full join is a type of outer join that combines the result-sets of both left and right joins.

> [!NOTE]  
> MySQL does not support Full Outer Join. Instead, you can imitate its working by performing union operation between the result-sets obtained from Left Join and Right Join.

Syntax:
```sql
SELECT column1, column2, ...
FROM table1
FULL JOIN table2
ON table1.column_name = table2.column_name;
```


