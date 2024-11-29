### Handling Duplicates

Handling duplicates in an SQL database becomes necessary to prevent the following consequences:
- The existence of duplicates in an organizational database will lead to logical errors.
- Duplicate data occupies space in the storage, which leads to decrease in usage efficiency of a database.
- Due to the increased use of resources, the overall cost of the handling resources rises.
- With increase in logical errors due to the presence of duplicates, the conclusions derived from data analysis in a database will also be erroneous.

Solution to handle duplicates:
- **Prevent Duplicates Entries**: using INSERT IGNORE, REPLACE INTO.
- **Remove Duplicates**: using DELETE, TRUNCATE, DROP.
- **Eliminating Duplicates from a Table**: using DISTINCT, GROUP BY, HAVING, ROW_NUMBER().