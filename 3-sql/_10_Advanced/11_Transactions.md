### Transactions

A transaction is a unit or sequence of work that is performed on a database. Transactions are accomplished in a logical order, whether in a manual fashion by a user or automatically by some sort of a database program.

A transaction is the propagation of one or more changes to the database. For example, if you are creating, updating or deleting a record from the table, then you are performing a transaction on that table. It is important to control these transactions to ensure the data integrity and to handle database errors.

Practically, you will club many SQL queries into a group and you will execute all of them together as a part of a transaction.

Properties of Transactions:
- **Atomicity**: It ensures that all operations within the work unit are completed successfully; otherwise, the transaction is aborted at the point of failure, and previous operations are rolled back to their former state.
- **Consistency**: It ensures that the database properly changes states upon a successfully committed transaction.
- **Isolation**: It enables transactions to operate independently of and transparent to each other.
- **Durability**: It ensures that the result or effect of a committed transaction persists in case of a system failure.

Transactional Control Commands:
- **BEGIN TRANSACTION**: It is used to start a new transaction. 
- **COMMIT**: It is used to save the transaction.
- **ROLLBACK**: It is used to undo the transactions that have not already been saved.
- **SAVEPOINT**: It is used to temporarily save a transaction so that you can roll back to that point whenever necessary.
- **SET TRANSACTION**: It is used to set the characteristics for the transaction.

1. `The Commit Command`: 

The COMMIT command is the transactional command used to save changes invoked by a transaction. It saves all the transactions occurred on the database since the last COMMIT or ROLLBACK.

Syntax:
```sql
COMMIT;
```

Example:
```sql
BEGIN TRANSACTION;
UPDATE employees
SET emp_salary = 70000
WHERE emp_id = 1;
COMMIT;
```

2. `The Rollback Command`:

The ROLLBACK command is the transactional command used to undo transactions that have not already been saved to the database. This command can only undo transactions since the last COMMIT or ROLLBACK.

Syntax:
```sql
ROLLBACK;
```

Example:
```sql
BEGIN TRANSACTION;
UPDATE employees
SET emp_salary = 70000
WHERE emp_id = 1;
ROLLBACK;
```

3. `The Savepoint Command`:

A SAVEPOINT is a logical rollback point in a transaction.

Usually, when you execute the ROLLBACK command, it undoes the changes until the last COMMIT. But, if you create save points you can partially roll the transaction back to these points. You can create multiple save points between two commits.

Syntax:
```sql
SAVEPOINT savepoint_name;
```

Example:
```sql

BEGIN TRANSACTION;
UPDATE employees
SET emp_salary = 70000
WHERE emp_id = 1;
SAVEPOINT point1;
UPDATE employees
SET emp_salary = 80000
WHERE emp_id = 2;
SAVEPOINT point2;
UPDATE employees
SET emp_salary = 90000
WHERE emp_id = 3;
ROLLBACK TO point1;
COMMIT;
```
