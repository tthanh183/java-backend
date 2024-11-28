### Select 

To work with a database in SQL, we need to first select the database we want to work with. After selecting the database, we can perform various operations on it such as creating tables, inserting data, updating data, and deleting data.

```sql
USE database_name;
```

Selecting a Non-Existent Database:
```sql
USE non_existent_database;
```
Output:
```sql
ERROR 1049 (42000): Unknown database 'non_existent_database'
```