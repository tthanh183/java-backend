### Using Joins

The SQL `Join` clause is used to combine data from two or more tables in a database. When the related data is stored across multiple tables, joins help you to retrieve records combining the fields from these tables using their foreign keys.

The part of the `Join` clause that specifies the columns on which records from two or more tables are joined is known as join-predicate. This predicate is usually specified along with the ON clause and uses various comparison operators such as, <, >, <>, <=, >=, !=, BETWEEN, LIKE, and NOT etc. We can also connect multiple join predicates with logical operators AND, OR, and NOT.

Syntax:
```sql
SELECT column1, column2, ...
FROM table1
JOIN table2
ON table1.column_name = table2.column_name;
```

Types of Joins:
- Inner Join: Returns records that have matching values in both tables.
- Left Join (or Left Outer Join): Returns all records from the left table and the matched records from the right table.
- Right Join (or Right Outer Join): Returns all records from the right table and the matched records from the left table.
- Full Join (or Full Outer Join): Returns all records when there is a match in either left or right table.
- Cross Join: Returns the Cartesian product of the two tables.
- Self Join: Joins a table with itself.
