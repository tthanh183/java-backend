### Auto Increment

The SQL Auto Increment is used to automatically add unique sequential values into a column of a table.

We usually define the Auto Increment on a column while creating a table. And when we insert new records into the table, the unique values are added to them.

1. **Auto Increment in MySQL:**

In MySQL, we can define the Auto Increment column by using the `AUTO_INCREMENT` attribute. The column with this attribute will automatically generate a unique value for each new record.

```sql
CREATE TABLE table_name (
    column1 datatype AUTO_INCREMENT,
    column2 datatype,
    ...
);
```

To insert a new record into the table, we don't need to specify the value for the Auto Increment column. MySQL will automatically generate a unique value for it.

```sql
INSERT INTO table_name (column2, ...)
VALUES (value2, ...);
```


2. **Auto Increment in SQL Server:**

In SQL Server, we can define the Auto Increment column by using the `IDENTITY` attribute. The column with this attribute will automatically generate a unique value for each new record.

```sql
CREATE TABLE table_name (
    column1 datatype IDENTITY(1,1),
    column2 datatype,
    ...
);
```
