### Relational Database Management System (RDBMS) Concepts

RDBMS stands for Relational Database Management System. RDBMS is the basis for SQL, and for all modern database systems like MS SQL Server, IBM DB2, Oracle, MySQL, and Microsoft Access.

1. **What is Table**

The data in an RDBMS is stored in database objects known as tables. This table is basically a collection of related data entries and it consists of numerous columns and rows.

```sql
+----+----------+-----+-----------+
| ID | NAME     | AGE | ADDRESS   |
+----+----------+-----+-----------+
|  1 | Ramesh   |  25 | Ahmedabad |
|  2 | Khilan   |  22 | Delhi     |
```
2. **What is Field**

Every table is broken up into smaller entities called fields. A field is a column in a table that is designed to maintain specific information about every record in the table.
```sql
+----+----------+-----+-----------+
| ID | NAME     | AGE | ADDRESS   |
+----+----------+-----+-----------+
```
3. **What is Record or a Row**

A record is also called as a row of data is each individual entry that exists in a table.
```sql
+----+----------+-----+-----------+
| ID | NAME     | AGE | ADDRESS   |
+----+----------+-----+-----------+
|  1 | Ramesh   |  25 | Ahmedabad |
```

4. **What is Column**

A column is a vertical entity in a table that contains all information associated with a specific field in a table.

```sql
+----+
| ID |
+----+
|  1 |
```

5. **What is NULL**

A NULL value in a table is a value in a field that appears to be blank, which means a field with a NULL value is a field with no value.

It is very important to understand that a NULL value is different than a zero value or a field that contains spaces. A field with a NULL value is the one that has been left blank during a record creation.

```sql
+----+----------+-----+-----------+
| ID | NAME     | AGE | ADDRESS   |
+----+----------+-----+-----------+
|  1 | Ramesh   |  25 |       |
```
6. **SQL Constraints**

Constraints are the rules enforced on data columns on a table. These are used to limit the type of data that can go into a table. This ensures the accuracy and reliability of the data in the database.

Constraints can either be column level or table level. Column level constraints are applied only to one column whereas, table level constraints are applied to the entire table.

| Constraint     | Description                                                                         |
|----------------|-------------------------------------------------------------------------------------|
| NOT NULL       | Ensures that a column cannot have a NULL value.                                     |
| UNIQUE         | Ensures that all values in a column are different.                                  |
| PRIMARY KEY    | A combination of a NOT NULL and UNIQUE. Uniquely identifies each row in a table.    |
| FOREIGN KEY    | Uniquely identifies a row/record in another table.                                  |
| CHECK          | Ensures that all values in a column satisfy certain conditions.                     |
| DEFAULT        | Sets a default value for a column when no value is specified.                       |
| INDEX          | Used to create and retrieve data from the database very quickly.                    |
| AUTO_INCREMENT | Automatically generates a unique number when a new record is inserted into a table. |

