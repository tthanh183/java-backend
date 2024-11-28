### Rename Views

There is no direct query to rename a view in SQL. In MySQL we can rename a view using the RENAME TABLE statement and in MS SQL Server we can rename a view using the sp_rename procedure.

In many cases, deleting the existing view and then re-creating it with a new name is rather recommended.

Syntax:

```sql
RENAME TABLE old_view_name TO new_view_name;
```

