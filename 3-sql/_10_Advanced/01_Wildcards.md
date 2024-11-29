### Wildcards

SQL Wildcards are special characters used as substitutes for one or more characters in a string. They are used with the LIKE operator in SQL, to search for specific patterns in character strings or compare various strings.

The LIKE operator in SQL is case-sensitive, so it will only match strings that have the exact same case as the specified pattern.

| Wildcard   | Description                                  |
|------------|----------------------------------------------|
| %          | Represents zero or more characters.          |
| _          | Represents a single character.               |

Syntax:
```sql
SELECT column1, column2, ...
FROM table_name
WHERE column_name LIKE [wildcard_pattern];
```
