### Date Time in SQL

SQL provides multiple datatypes and functions to handle Date and Time values in a database. This is because Date and Time values are represented in various formats. For instance, there are two common ways to represent a date value: DD/MM/YYYY and MM/DD/YYYY. Similarly, there is more than a single way to represent time values.

Date and Time Datatypes in SQL:

| Datatype  | Description                                                   |
|-----------|---------------------------------------------------------------|
| DATE      | Represents a date in the format YYYY-MM-DD.                   |
| TIME      | Represents a time in the format HH:MM:SS.                     |
| DATETIME  | Represents a date and time in the format YYYY-MM-DD HH:MM:SS. |
| TIMESTAMP | Represents a timestamp in the format YYYY-MM-DD HH:MM:SS.     |
| YEAR      | Represents a year in the format YYYY.                         |
    
Date and Time Functions in SQL:

| Function            | Description                                      |
|---------------------|--------------------------------------------------|
| CURRENT_DATE()      | Returns the current date.                        |
| CURRENT_TIME()      | Returns the current time.                        |
| CURRENT_TIMESTAMP() | Returns the current date and time.               |
| EXTRACT()           | Extracts a part of a date or time value.         |
| DATE_ADD()          | Adds a specified time interval to a date.        |
| DATE_SUB()          | Subtracts a specified time interval from a date. |
| DATEDIFF()          | Returns the difference between two dates.        |
| DATE_FORMAT()       | Formats a date value.                            |
| DAYNAME()           | Returns the name of the day of the week.         |
| DAYOFMONTH()        | Returns the day of the month.                    |
    
