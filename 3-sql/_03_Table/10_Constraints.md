### Constraints 

SQL `Constraints` are the rules applied to a data columns or the complete table to limit the type of data that can go into a table. When you try to perform any `INSERT`, `UPDATE`, or `DELETE` operation on the table, RDBMS will check whether that data violates any existing `constraints` and if there is any violation between the defined `constraint` and the data action, it aborts the operation and returns an error.

Syntax:
```sql
CREATE TABLE table_name (
    column_name data_type constraint_name,
    ...
);
```

- `NOT NULL` Constraint: 

When applied to a column, NOT NULL constraint ensure that a column cannot have a NULL value.

```sql
CREATE TABLE employees (
    id INT NOT NULL,
    name VARCHAR(50) NOT NULL,
    age INT NOT NULL,
    address VARCHAR(255)
);
```

- `UNIQUE` Constraint:

When applied to a column, UNIQUE Key constraint ensure that a column accepts only UNIQUE values.
```sql
CREATE TABLE employees (
    id INT PRIMARY KEY,
    name VARCHAR(50) UNIQUE,
    age INT,
    address VARCHAR(255)
);
```

- `DEFAULT` Constraint:

When applied to a column, DEFAULT constraint provides a default value for a column when no value is specified.
```sql
CREATE TABLE employees (
    id INT PRIMARY KEY,
    name VARCHAR(50),
    age INT DEFAULT 18,
    address VARCHAR(255)
);
```

- `PRIMARY KEY` Constraint:

When applied to a column, PRIMARY Key constraint ensure that a column accepts only UNIQUE value and there can be a single PRIMARY Key on a table but multiple columns can constituet a PRIMARY Key.

```sql

CREATE TABLE employees (
    id INT PRIMARY KEY,
    name VARCHAR(50),
    age INT,
    address VARCHAR(255)
);
```

- `FOREIGN KEY` Constraint:

FOREIGN Key constraint maps with a column in another table and uniquely identifies a row/record in that table.
```sql
CREATE TABLE employees (
    id INT PRIMARY KEY,
    name VARCHAR(50),
    age INT,
    address VARCHAR(255),
    department_id INT,
    FOREIGN KEY (department_id) REFERENCES departments (id)
);
```

- `CHECK` Constraint:

When applied to a column, CHECK Value constraint works like a validation and it is used to check the validity of the data entered into the particular column of the table.

```sql
CREATE TABLE employees (
    id INT PRIMARY KEY,
    name VARCHAR(50),
    age INT,
    address VARCHAR(255),
    salary DECIMAL CHECK (salary > 0)
);
```

- `INDEX` Constraint:

The INDEX constraints are created to speed up the data retrieval from the database. An Index can be created by using a single or group of columns in a table. A table can have a single PRIMARY Key but can have multiple INDEXES. An Index can be Unique or Non Unique based on requirements.

```sql
CREATE TABLE employees (
    id INT PRIMARY KEY,
    name VARCHAR(50),
    age INT,
    address VARCHAR(255),
    INDEX name_index (name)
);
```

- `DROP` Constraint:

```sql
ALTER TABLE table_name DROP CONSTRAINT constraint_name;
```