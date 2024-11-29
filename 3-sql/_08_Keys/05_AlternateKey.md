### Alternate Key

SQL Alternate Keys in a database table are candidate keys that are not currently selected as a primary key. They can be used to uniquely identify a tuple(or a record) in a table.

There is no specific query or syntax to set the alternate key in a table. It is just a column that is a close second candidate which could be selected as a primary key. Hence, they are also called secondary candidate keys.

Features of Alternate Key:
- The alternate key does not allow duplicate values.
- A table can have more than one alternate keys.
- The alternate key can contain NULL values unless the NOT NULL constraint is set explicitly.
- All alternate keys can be candidate keys, but all candidate keys can not be alternate keys.
- The primary key, which is also a candidate key, can not be considered as an alternate key.