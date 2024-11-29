### Stored Procedures

An SQL stored procedure is a group of pre-compiled SQL statements (prepared SQL code) that can be reused by simply calling it whenever needed.

It can be used to perform a wide range of database operations such as inserting, updating, or deleting data, generating reports, and performing complex calculations. Stored procedures are very useful because they allow you to encapsulate (bundle) a set of SQL statements as a single unit and execute them repeatedly with different parameters, making it easy to manage and reuse the code.

> [!NOTE]  
> Procedures have similar structure as functions: they accept parameters and perform operations when we call them. But, the difference between them is that SQL stored procedures are simpler to write or create, whereas functions have a more rigid structure and support fewer clauses.
1. **Syntax to create a stored procedure**:

```sql
DELIMITER //
CREATE PROCEDURE procedure_name (IN parameter1 datatype, IN parameter2 datatype, ...)
BEGIN
    -- SQL statements
END //
DELIMITER ;
```

> [!NOTE]  
> - The CREATE PROCEDURE statement is used to create the procedure. We can define any number of input parameters as per the requirement.
> - The SQL statements that make up the procedure are placed between the BEGIN and END keywords.

2. **Parameters**:

- IN: It is used to pass the value to the procedure.

Example:

```sql
DELIMITER //
CREATE PROCEDURE getEmployee(IN emp_id INT)
BEGIN
    SELECT * FROM employees WHERE employee_id = emp_id;
END //
DELIMITER ;
```

```sql
CALL getEmployee(1);
```

The above procedure will return the details of the employee with `employee_id` 1.

- OUT: It is used to return the value from the procedure.

Example:

```sql
DELIMITER //
CREATE PROCEDURE getEmployee(IN emp_id INT, OUT emp_name VARCHAR(50))
BEGIN
    SELECT employee_name INTO emp_name FROM employees WHERE employee_id = emp_id;
END //
DELIMITER ;
```

```sql
CALL getEmployee(1, @emp_name);
SELECT @emp_name;
```

The above procedure will return the name of the employee with `employee_id` 1.  

| emp_id        |
|---------------|
| Alice Johnson |

- INOUT: It is used to pass the value to the procedure and return the value from the procedure.

Example:

```sql
DELIMITER //
CREATE PROCEDURE getEmployee(INOUT emp_id INT)
BEGIN
    SELECT employee_name INTO emp_id FROM employees WHERE employee_id = emp_id;
END //
DELIMITER ;
```

```sql
SET @emp_id = 1;
CALL getEmployee(@emp_id);
SELECT @emp_id;
```

The above procedure will return the name of the employee with `employee_id` 1.

| emp_id        |
|---------------|
| Alice Johnson |

