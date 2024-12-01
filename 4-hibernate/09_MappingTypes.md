### Mapping Types

When you prepare a Hibernate mapping document, you find that you map the Java data types into RDBMS data types. The types declared and used in the mapping files are not Java data types; they are not SQL database types either. These types are called Hibernate mapping types, which can translate from Java to SQL data types and vice versa.

The following table lists the most commonly used Hibernate mapping types:

- Primitive Types

| Mapping Type | Java Type  | SQL Type |
|--------------|------------|----------|
| int          | Integer    | INTEGER  |
| long         | Long       | BIGINT   |
| short        | Short      | SMALLINT |
| float        | Float      | FLOAT    |
| double       | Double     | DOUBLE   |
| big_decimal  | BigDecimal | NUMERIC  |
| char         | Character  | CHAR     |
| byte         | Byte       | TINYINT  |
| boolean      | Boolean    | BIT      |
| yes_no       | Boolean    | CHAR(1)  |
| true_false   | Boolean    | CHAR(1)  |

- Date and Time Types

| Mapping Type  | Java Type          | SQL Type  |
|---------------|--------------------|-----------|
| date          | java.util.Date     | DATE      |
| time          | java.util.Date     | TIME      |
| timestamp     | java.util.Date     | TIMESTAMP |
| calendar      | java.util.Calendar | TIMESTAMP |
| calendar_date | java.util.Calendar | DATE      |
| calendar_time | java.util.Calendar | TIME      |

- Binary or Large Object Types

| Mapping Type | Java Type | SQL Type  |
|--------------|-----------|-----------|
| binary       | byte[]    | VARBINARY |
| text         | String    | CLOB      |
| blob         | byte[]    | BLOB      |
| clob         | String    | CLOB      |
