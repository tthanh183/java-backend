### Introduction to SQL

SQL (Structured Query Language) is a programming language which is used to manage data stored in relational databases like MySQL, MS Access, SQL Server, Oracle, Sybase, Informix, Postgres etc.    

1. **SQL Basic Commands**

We have a list of standard SQL commands to interact with relational databases, These commands are CREATE, SELECT, INSERT, UPDATE, DELETE, DROP and TRUNCATE and can be classified into the following groups based on their nature −

- **DDL (Data Definition Language)**: A Data Definition Language (DDL) is a computer language which is used to create and modify the structure of database objects which include tables, views, schemas, and indexes etc.

| Command  | Description                                                                                   |
|----------|-----------------------------------------------------------------------------------------------|
| CREATE   | Creates a new table, a view of a table, or other object in the database.                      |
| ALTER    | Modifies an existing database object, such as a table.                                        |
| DROP     | Deletes an entire table, a view of a table or other object in the database.                   |
| TRUNCATE | Removes all records from a table, including all spaces allocated for the records are removed. |

- **DML (Data Manipulation Language)**: A Data Manipulation Language (DML) is a computer programming language which is used for adding, deleting, and modifying data in a database.

| Command | Description                                        |
|---------|----------------------------------------------------|
| SELECT  | Retrieves certain records from one or more tables. |
| INSERT  | Creates a record.                                  |
| UPDATE  | Modifies records.                                  |
| DELETE  | Deletes records.                                   |

- **TCL (Transaction Control Language)**: Transaction Control Language (TCL) is a computer language which is used to manage different transactions occurring within a database.

| Command   | Description                              |
|-----------|------------------------------------------|
| COMMIT    | Saves the changes.                       |
| ROLLBACK  | Restores the database to original state. |
| SAVEPOINT | Identifies a point in a transaction.     |

- **DCL (Data Control Language)**: Data Control Language (DCL) is a computer programming language which is used to control access to data stored in a database.

| Command | Description                              |
|---------|------------------------------------------|
| GRANT   | Gives a privilege to user.               |
| REVOKE  | Takes back privileges granted from user. |

2. **SQL Data Types**