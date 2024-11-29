### Introduction to Indexes

SQL Indexes are special lookup tables that are used to speed up the process of data retrieval. They hold pointers that refer to the data stored in a database, which makes it easier to locate the required data records in a database table.

While an index speeds up the performance of data retrieval queries (SELECT statement), it slows down the performance of data input queries (UPDATE and INSERT statements). However, these indexes do not have any effect on the data.

SQL Indexes need their own storage space within the database. Despite that, the users cannot view them physically as they are just performance tools.

Create an Index:
```sql
CREATE INDEX index_name ON table_name (column_name);
```

Types of Indexes:
- Single Column Index
- Composite Index
- Implicit Index
- Unique Index

1. **Unique Index**:

Unique indexes are used not only for performance, but also for data integrity. A unique index does not allow any duplicate values to be inserted into the table. It is automatically created by PRIMARY and UNIQUE constraints when they are applied on a database table, in order to prevent the user from inserting duplicate values into the indexed table column(s).

Create a Unique Index:
```sql
CREATE UNIQUE INDEX index_name ON table_name (column_name);
```
2. **Single-Column Index**:

A single-column index is created only on one table column. The syntax is as follows.

Create a Single-Column Index:
```sql
CREATE INDEX index_name ON table_name (column_name);
```
3. **Composite Index**:

A composite index is an index that can be created on two or more columns of a table. Its basic syntax is as follows.

Create a Composite Index:
```sql
CREATE INDEX index_name ON table_name (column1, column2);
```

4. **Implicit Index**:

Implicit indexes are indexes that are automatically created by the database server when an object is created. For example, indexes are automatically created when primary key and unique constraints are created on a table in MySQL database.

5. **Drop an Index**:

To drop an index, use the following syntax.
```sql
DROP INDEX index_name ON table_name;
```
