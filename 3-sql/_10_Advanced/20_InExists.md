### In And Exists

1. **In**:

The IN operator in SQL is used to check if a particular value matches any within a given set. This set of values can be specified individually or obtained from a subquery. We can use the IN operator with the WHERE clause to simplify queries and reduce the use of multiple OR conditions.

2. **Exists**:

The EXISTS operator is used to look for the existence of a row in a given table that satisfies a set of criteria. It is a Boolean operator that compares the result of the subquery to an existing record and returns true or false.

The returned value is true, if the subquery fetches single or multiple records; and false, if no record is matched. EXISTS operator follows the querys efficiency features, i.e. when the first true event is detected, it will automatically stop processing further.

We can use the EXISTS operator with the SELECT, UPDATE, INSERT and DELETE queries.