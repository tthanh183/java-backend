### Rename Table

SQL provides two ways to rename an MySQL table. You can use either SQL RENAME TABLE or ALTER TABLE statement to change a table name in MySQL RDBMS.

1. **RENAME TABLE Statement**:

```sql
RENAME TABLE old_table_name TO new_table_name;
```

2. **ALTER TABLE Statement**:

```sql
ALTER TABLE old_table_name RENAME [TO|AS] new_table_name;
```

3. **Rename Table in SQL Server**:

In SQL Server, you can use the `sp_rename` system stored procedure to rename a table.

```sql
EXEC sp_rename 'old_table_name', 'new_table_name';
```