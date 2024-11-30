### Database Tuning

We can implement the following techniques to optimize the performance of a database:

1. **Database Normalization**:

Normalization is the process of removing of duplicate data from a database. We can normalize a database by breaking down larger tables into smaller related tables. This increases the performance of database as it requires less time to retrieve data from small tables instead of one large table.

2. **Indexing**:

In SQL, indexes are the pointers (memory address) to the location of specific data in database. We use indexes in our database to reduce query time, as the database engine can jump to the location of a specific record using its index instead of scanning the entire database.

3. **Query Optimization**:

- Use Select fields instead of Select

In large databases, we should always retrieve only the required columns from the database instead of retrieving all the columns, even when they are not needed. We can easily do this by specifying the column names in the SELECT statement instead of using the SELECT (*) statement.

- Use wildcards at the end of a string

Wildcards (%) are characters that we use to search for data based on patterns. These wildcards paired with indexes only improves performance because the database can quickly find the data that matches the pattern.

- Use Joins instead of Subqueries

SQL JOINs are used to combine two tables based on a common column. There are two ways of creating a JOIN implicit join and explicit join. Explicit Join notation use the JOIN keyword with the ON clause to join two tables while the implicit join notation does not use the JOIN keyword and works with the WHERE clause.

- Avoid using Select DISTINCT

The DISTINCT operator in SQL is used to retrieve unique records from the database. And on a properly designed database table with unique indexes, we rarely use it.

But, if we still have to use it on a table, using the GROUP BY clause instead of the DISTINCT keyword shows a better query performance (at least in some databases).

- Use UNION ALL instead of UNION

The OR operator is used to combine multiple conditions when filtering a database. Whenever we use OR in a filter condition each statement is processed separately. This degrades database performance as the entire table must be scanned multiple times to retrieve the data that matches the filter condition.

Instead, we can use a more optimized solution; by breaking the different OR conditions into separate queries, which can be processed parallelly by the database. Then, the results from these queries can be combined using UNION.

- Use WHERE instead of HAVING

The WHERE and HAVING clause are both used to filter data in SQL. However, WHERE clause is more efficient than HAVING. With WHERE clause, only the records that match the condition are retrieved. But with HAVING clause, it first retrieves all the records and then filters them based on a condition. Therefore, the WHERE clause is preferable.