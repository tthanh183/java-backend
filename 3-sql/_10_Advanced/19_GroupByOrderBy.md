### Group By And Order By

1. **Group By**:

Using the GROUP BY clause we can organize the data in a table into groups (based on a column) and perform required calculations on them.

This clause is often used with the aggregate functions such as MIN(), MAX(), SUM(), AVG(), and COUNT() etc.

It is often used with the SELECT statement, and it is placed after the WHERE clause or before the HAVING clause. If we use the Order By clause, the Group By clause should precede the Order By clause.

Syntax:
```sql
SELECT column1, column2, aggregate_function(column3)
FROM table_name
WHERE condition
GROUP BY column1, column2
ORDER BY column1, column2;
```

2. **Order By** is used to sort the result set by one or more columns.

The ORDER BY clause is used to sort the query results. This clause is used at the end of a SELECT statement, following the WHERE, HAVING and GROUP BY clauses. We can sort the table column in ascending or descending order with the by specifying the sort order as ASC and DESC respectively. If we do not specify any order, it defaults to ascending order.

Syntax:
```sql
SELECT column1, column2, column3
FROM table_name
ORDER BY column1, column2 ASC|DESC;
```
