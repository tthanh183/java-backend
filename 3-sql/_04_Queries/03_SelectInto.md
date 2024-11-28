### Select ... Into

The The SQL SELECT INTO Statement creates a new table and inserts data from an existing table into the newly created table. The new table is automatically created based on the structure of the columns in the SELECT statement and can be created in the same database or in a different database.

However, it's important to note that the SELECT INTO statement does not preserve any indexes, constraints, or other properties of the original table, and the new table will not have any primary keys or foreign keys defined by default. Therefore, you may need to add these properties to the new table manually if necessary.

> [!NOTE]  
> MySQL does not support the SELECT INTO statement. Instead, you can use the CREATE TABLE ... SELECT statement to achieve the same result.

Syntax:
```sql
SELECT column1, column2, ...
INTO new_table_name [IN external_database]
FROM existing_table_name
WHERE condition;
```
