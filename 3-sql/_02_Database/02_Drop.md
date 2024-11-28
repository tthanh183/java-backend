### Drop Database

The SQL DROP DATABASE statement is used to delete an existing database along with all the data such as tables, views, indexes, stored procedures, and constraints.

- Syntax:
```sql
DROP DATABASE database_name;
```

- Drop Database If Exists:
```sql
DROP DATABASE IF EXISTS database_name;
```

- Drop Database That Does Not Exist:
```sql
DROP DATABASE IF EXISTS database_name;
```
Output:
```sql
Query OK, 0 rows affected (0.00 sec)
```

- Drop Multiple Databases:
```sql
DROP DATABASE IF EXISTS database_name1, database_name2, database_name3;
```
