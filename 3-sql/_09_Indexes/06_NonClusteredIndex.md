### Non Clustered Index

The SQL Non-Clustered index is similar to the Clustered index. When defined on a column, it creates a special table which contains the copy of indexed columns along with a pointer that refers to the location of the actual data in the table. However, unlike Clustered indexes, a Non-Clustered index cannot physically sort the indexed columns.

> [!NOTE]  
> The non-clustered indexes are a type of index used in databases to speed up the execution time of database queries.
> These indexes require less storage space than clustered indexes because they do not store the actual data rows.
> We can create multiple non-clustered indexes on a single table.

To get a better understanding, look at the following figure illustrating the working of non-clustered indexes
![Non-Clustered Index](https://www.tutorialspoint.com/sql/images/non-clustered.jpg)

Syntax:
```sql
CREATE NONCLUSTERED INDEX index_name ON table_name (column_name);
```
