### Null Function

SQL NULL functions are used to perform operations on NULL values that are stored in the database tables.

A NULL value serves as a placeholder in the database when data is absent or the required information is unavailable. It is a flexible value not associated to any specific data type and can be used in columns of various data types, including string, int, varchar, and more.

Following are the various features of a NULL value:
- The NULL value is different from a zero value or a field containing a space. A record with a NULL value is one that has been left empty or unspecified during record creation.
- The NULL value assists us in removing ambiguity from data. Thus, maintaining the uniform datatype across the column.

Some of the SQL NULL functions are:
- `IS NULL`: This function is used to check if a value is NULL or not. If the value is NULL, it returns true; otherwise, it returns false.
- `IS NOT NULL`: This function is used to check if a value is not NULL. If the value is not NULL, it returns true; otherwise, it returns false.
- `COALESCE()`: This function is used to return the first non-NULL value from the list of arguments. If all the arguments are NULL, it returns NULL.
- `NULLIF()`: This function is used to compare two expressions. If the two expressions are equal, it returns NULL; otherwise, it returns the first expression.
- `IFNULL()`: This function is used to return the second expression if the first expression is NULL; otherwise, it returns the first expression.

