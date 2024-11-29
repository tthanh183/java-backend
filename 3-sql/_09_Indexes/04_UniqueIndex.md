### Unique Index

The SQL Unique Index ensures that no two rows in the indexed columns of a table have the same values (no duplicate values allowed).

A unique index can be created on one or more columns of a table using the CREATE UNIQUE INDEX statement in SQL.

> [!NOTE]  
> If the unique index is only created on a single column, the rows in that column will be unique.
> If a single column contains NULL in multiple rows, we cannot create a unique index on that column.
> If the unique index is created on multiple columns, the combination of rows in these columns will be unique.
> We cannot create a unique index on multiple columns if the combination of columns contains NULL in more than one row.

Syntax:
```sql
CREATE UNIQUE INDEX index_name ON table_name (column_name);
```
