### Sub Queries

An SQL Subquery, is a SELECT query within another query. It is also known as Inner query or Nested query and the query containing it is the outer query.

The outer query can contain the SELECT, INSERT, UPDATE, and DELETE statements. We can use the subquery as a column expression, as a condition in SQL clauses, and with operators like =, >, <, >=, <=, IN, BETWEEN, etc.

Rules for Subqueries:
- Subqueries must be enclosed within parentheses.
- Subqueries can be nested within another subquery.
- A subquery must contain the SELECT query and the FROM clause always.
- A subquery consists of all the clauses an ordinary SELECT clause can contain: GROUP BY, WHERE, HAVING, DISTINCT, TOP/LIMIT, etc. However, an ORDER BY clause is only used when a TOP clause is specified. It can't include COMPUTE or FOR BROWSE clause.
- A subquery can return a single value, a single row, a single column, or a whole table. They are called scalar subqueries.

Syntax:
```sql
SELECT column1, column2, ...
FROM table_name
WHERE column_name operator (SELECT column_name FROM table_name WHERE condition);
```
