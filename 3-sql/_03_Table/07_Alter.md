### Alter Table

The SQL ALTER TABLE command is a part of Data Definition Language (DDL) and modifies the structure of a table. The ALTER TABLE command can add or delete columns, create or destroy indexes, change the type of existing columns, or rename columns or the table itself.

Syntax:
```sql
ALTER TABLE table_name [alter_option ...];
```

- Add Column:
```sql
ALTER TABLE table_name ADD column_name column_definition;
```

Example:
```sql
ALTER TABLE employees ADD email VARCHAR(50);
```

- Drop Column:
```sql
ALTER TABLE table_name DROP COLUMN column_name;
```

Example:
```sql
ALTER TABLE employees DROP COLUMN address;
```

- Add Index:
```sql
ALTER TABLE table_name ADD INDEX index_name (column_name);
```

Example:
```sql
ALTER TABLE employees ADD INDEX name_index (name);
```

- Drop Index:
```sql
ALTER TABLE table_name DROP INDEX index_name;
```

Example:
```sql
ALTER TABLE employees DROP INDEX name_index;
```

- Add Primary Key:
```sql
ALTER TABLE table_name ADD PRIMARY KEY (column_name);
```

Example:
```sql
ALTER TABLE employees ADD PRIMARY KEY (id);
```

- Drop Primary Key:
```sql
ALTER TABLE table_name DROP PRIMARY KEY;
```

Example:
```sql
ALTER TABLE employees DROP PRIMARY KEY;
```

- Add Constraint:
```sql
ALTER TABLE table_name ADD CONSTRAINT constraint_name constraint_definition;
```

Example:
```sql
ALTER TABLE employees ADD CONSTRAINT fk_department_id FOREIGN KEY (department_id) REFERENCES departments (id);
```

- Drop Constraint:
```sql
ALTER TABLE table_name DROP CONSTRAINT constraint_name;
```

Example:
```sql
ALTER TABLE employees DROP CONSTRAINT fk_department_id;
```

- Rename Table:
```sql
ALTER TABLE old_table_name RENAME TO new_table_name;
```

Example:
```sql
ALTER TABLE employees RENAME TO staff;
```

- Modify Column:
```sql
ALTER TABLE table_name MODIFY column_name column_definition;
```

Example:
```sql
ALTER TABLE employees MODIFY name VARCHAR(100);
```
