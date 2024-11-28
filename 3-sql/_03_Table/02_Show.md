### Show Databases

There are several instances when you need to retrieve a list of tables from your database

1. **MySQL**: 

You can use the `SHOW DATABASES` command to list all the databases in the MySQL server.

Example:
```sql
USE mysql;
SHOW DATABASES;
```

2. **SQL Server**:

You can use the `SELECT name FROM sys.databases` command to list all the databases.

Example:
```sql
SELECT name FROM sys.databases;
```
