### Foreign Key

In SQL, a Foreign Key is a column in one table that matches a Primary Key in another table, allowing the two tables to be connected together.

A foreign key also maintains referential integrity between two tables, making it impossible to drop the table containing the primary key (preserving the connection between the tables).

Syntax:
```sql
CREATE TABLE table_name (
    column1 datatype,
    column2 datatype,
    ...
    FOREIGN KEY (column_name) REFERENCES other_table_name(column_name)
);
```

Create Foreign Key on Existing Table:
```sql
ALTER TABLE table_name ADD FOREIGN KEY (column_name) REFERENCES other_table_name(column_name);
```

Drop Foreign Key:
```sql
ALTER TABLE table_name DROP FOREIGN KEY FOREIGN_KEY_NAME;
```
