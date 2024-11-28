### Rename Database

If you want to rename a database, you can use the `RENAME DATABASE` statement or using `Dump` and `Reimport` the database.

1. 'RENAME DATABASE' Statement:
```sql
RENAME DATABASE old_database_name TO new_database_name;
```

> [!NOTE]  
> The `RENAME DATABASE` statement was used to rename a database in MySQL. It was removed in MySQL 5.1.23 because it was found to be dangerous and was seldom used.

> [!NOTE]  
> The `Alter Database` statement is used to rename a database in SQL Server, it is not supported in MySQL.

2. Dump and Reimport:
```sql
mysqldump -u root -p old_database_name > old_database_name.sql
mysql -u root -p -e "CREATE DATABASE new_database_name"
mysql -u root -p new_database_name < old_database_name.sql
```
