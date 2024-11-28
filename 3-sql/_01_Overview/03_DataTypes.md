### Data Types

An SQL data type refers to the type of data which can be stored in a column of a database table. In a column, the user can store numeric, string, binary, etc by defining data types. 

> [!NOTE]  
> While creating a database table in a database, we need to specify following two attributes to define a table column:
> - Column Name
> - Data Type

Data types can be different based on the database system. Here are some common data types used in MySQL:

| Data Type          | Description                                                       |
|--------------------|-------------------------------------------------------------------|
| `CHAR(size)`       | Fixed-length character string. The size is specified in bytes.    |
| `VARCHAR(size)`    | Variable-length character string. The size is specified in bytes. |
| `BINARY(size)`     | Fixed-length binary string. The size is specified in bytes.       |
| `VARBINARY(size)`  | Variable-length binary string. The size is specified in bytes.    |
| `TINYTEXT`         | Holds a string with a maximum length of 255 bytes.                |
| `TEXT(size)`       | Holds a string with a maximum length of 65,535 bytes.             |
| `LONGTEXT(size)`   | Holds a string with a maximum length of 4,294,967,295 bytes.      |
| `BLOB(size)`       | Holds a maximum of 65,535 bytes.                                  |
| `MEDIUMBLOB(size)` | Holds a maximum of 16,777,215 bytes.                              |
| `LONGBLOB(size)`   | Holds a maximum of 4,294,967,295 bytes.                           |
| `INT(size)`        | A normal-sized integer that can be signed or unsigned.            |
| `SMALLINT(size)`   | A small integer that can be signed or unsigned.                   |
| `BIGINT(size)`     | A large integer that can be signed or unsigned.                   |
| `FLOAT(size,d)`    | A floating-point number that cannot be unsigned.                  |
| `DOUBLE(size,d)`   | A double-precision floating-point number that cannot be unsigned. |
| `DECIMAL(size,d)`  | An exact fixed-point number.                                      |
| `DATE`             | A date value in the format YYYY-MM-DD.                            |
| `TIME`             | A time value in the format HH:MM:SS.                              |
| `DATETIME`         | A date and time value in the format YYYY-MM-DD HH:MM:SS.          |
| `TIMESTAMP`        | A timestamp value in the format YYYY-MM-DD HH:MM:SS.              |
| `YEAR`             | A year value in the format YYYY.                                  |
| ...                | ...                                                               |
