### Alias (AS)

You can rename a table or a column in a database temporarily by giving them another pseudo name. This pseudo name is known as Alias. The use of aliases is to address a specific table or a column in an SQL statement without changing their original name in the database. Aliases are created with the AS keyword.

Aliases can be especially useful when working with complex queries involving multiple tables or columns with similar names. By assigning temporary names to these tables or columns, you can make your SQL query more readable and easier to understand.

Syntax:
```sql
SELECT column1, column2, ...
FROM table_name AS alias_name;
```
