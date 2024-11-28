### Drop Views

The SQL DROP VIEW statement is used to delete an existing view, along with its definition and other information. Once the view is dropped, all the permissions for it will also be removed. We can also drop indexed views with this statement.

Suppose a table is dropped using the DROP TABLE command and it has a view associated to it, this view must also be dropped explicitly using the DROP VIEW command.

Syntax:
```sql
DROP VIEW view_name;
```

