### Clustered Index 

A clustered index in SQL is a type of index that determines the physical order in which the data values will be stored in a table.

When a clustered index is defined on a specific column, during the creation of a new table, the data is inserted into that column in a sorted order. This helps in faster retrieval of data since it is stored in a specific order.

> [!NOTE]  
> - It is recommended to have only one clustered index on a table. If we create multiple clustered indexes on the same table, the table will have to store the same data in multiple orders which is not possible.
> - When we try to create a primary key constraint on a table, a unique clustered index is automatically created on the table. However, the clustered index is not the same as a primary key. A primary key is a constraint that imposes uniqueness on a column or set of columns, while a clustered index determines the physical order of the data in the table.

> [!TIP]  
> MySQL database does not have a separate provisions for Clustered and Non-Clustered indexes. Clustered indexes are automatically created when PRIMARY KEY is defined on a table. And when the PRIMARY KEY is not defined, the first UNIQUE NOT NULL key is treated as a Clustered index.

Syntax:
```sql
CREATE CLUSTERED INDEX index_name ON table_name (column_name ASC|DESC);
```
