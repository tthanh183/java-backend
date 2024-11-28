### Backup Database

In this SQL Backup Database tutorial, we will explain how we can take a backup of a database in MySQL and MS SQL Server. It is very important and basic development practice to have a backup of the database in case the original is corrupted or lost due to power surges or disk crashes etc. By practicing this, the database can be recovered as it was before the failure.

1. **Backup Database in MySQL**

- Backup syntax:
```sql
mysqldump -u root -p database_name > database_name.sql
```
- Restore syntax:
```sql
mysql -u root -p -e "CREATE DATABASE database_name"
mysql -u root -p database_name < database_name.sql
```

2. **Backup Database in MS SQL Server**

- Backup syntax:
```sql
BACKUP DATABASE database_name TO DISK = 'C:\path\to\backup\database_name.bak'
```
- Restore syntax:
```sql
RESTORE DATABASE database_name FROM DISK = 'C:\path\to\backup\database_name.bak'
```
