### Mapping Date and Time

1. **Overview**

In this tutorial, we’ll show how to map temporal column values in Hibernate, including the classes from java.sql, java.util and java.time packages.

2. **Time Zone setup**:

When dealing with dates, it’s a good idea to set a specific time zone for the JDBC driver. This way, our application will be independent of the current time zone of the system.

For our example, we’ll set it up on a per-session basis:
    
```
session = HibernateUtil.getSessionFactory().withOptions()
  .jdbcTimeZone(TimeZone.getTimeZone("UTC"))
  .openSession();
```

Another way would be to set up the hibernate.jdbc.time_zone property in the Hibernate properties file that is used to construct the session factory. This way, we could specify the time zone once for the entire application.

3. **Mapping java.sql Types**:

The java.sql package contains JDBC types that are aligned with the types defined by the SQL standard:

- Date corresponds to the DATE SQL type, which is only a date without time.
- Time corresponds to the TIME SQL type, which is a time of the day specified in hours, minutes and seconds.
- Timestamp includes information about date and time with precision up to nanoseconds and corresponds to the TIMESTAMP SQL type.
These types are in line with SQL, so their mapping is relatively straightforward.

We can use either the @Basic or the @Column annotation:

```java
@Entity
public class TemporalValues {

    @Basic
    private java.sql.Date sqlDate;

    @Basic
    private java.sql.Time sqlTime;

    @Basic
    private java.sql.Timestamp sqlTimestamp;

}
```
We could then set the corresponding values:

```
temporalValues.setSqlDate(java.sql.Date.valueOf("2017-11-15"));
temporalValues.setSqlTime(java.sql.Time.valueOf("15:30:14"));
temporalValues.setSqlTimestamp(
  java.sql.Timestamp.valueOf("2017-11-15 15:30:14.332"));
```

Note that choosing java.sql types for entity fields may not always be a good choice. These classes are JDBC-specific and contain a lot of deprecated functionality.

4. **Mapping java.util.Date Types**:

The type java.util.Date contains both date and time information, up to millisecond precision. But it doesn’t directly relate to any SQL type.

This is why we need another annotation to specify the desired SQL type:

```java
@Basic
@Temporal(TemporalType.DATE)
private java.util.Date utilDate;

@Basic
@Temporal(TemporalType.TIME)
private java.util.Date utilTime;

@Basic
@Temporal(TemporalType.TIMESTAMP)
private java.util.Date utilTimestamp;
```

The @Temporal annotation has the single parameter value of type TemporalType. It can be either DATE, TIME or TIMESTAMP, depending on the underlying SQL type that we want to use for the mapping.

We could then set the corresponding fields:

```
temporalValues.setUtilDate(
  new SimpleDateFormat("yyyy-MM-dd").parse("2017-11-15"));
temporalValues.setUtilTime(
  new SimpleDateFormat("HH:mm:ss").parse("15:30:14"));
temporalValues.setUtilTimestamp(
  new SimpleDateFormat("yyyy-MM-dd HH:mm:ss.SSS")
    .parse("2017-11-15 15:30:14.332"));
```

As we’ve seen, the java.util.Date type (milliseconds precision) is not precise enough to handle the Timestamp value (nanoseconds precision).

So, when we retrieve the entity from the database, we’ll unsurprisingly find a java.sql.Timestamp instance in this field, even if we initially persisted a java.util.Date:

```
temporalValues = session.get(TemporalValues.class, 
  temporalValues.getId());
assertThat(temporalValues.getUtilTimestamp())
  .isEqualTo(java.sql.Timestamp.valueOf("2017-11-15 15:30:14.332"));
```

This should be fine for our code since Timestamp extends Date.

5. **Mapping java.time.Calendar Types**:

As with the java.util.Date, the java.util.Calendar type may be mapped to different SQL types, so we have to specify them with @Temporal.

The only difference is that Hibernate does not support mapping Calendar to TIME:

```java
@Basic
@Temporal(TemporalType.DATE)
private java.util.Calendar calendarDate;

@Basic
@Temporal(TemporalType.TIMESTAMP)
private java.util.Calendar calendarTimestamp;
```

Here’s how we can set the value of the field:

```
Calendar calendarDate = Calendar.getInstance(
  TimeZone.getTimeZone("UTC"));
calendarDate.set(Calendar.YEAR, 2017);
calendarDate.set(Calendar.MONTH, 10);
calendarDate.set(Calendar.DAY_OF_MONTH, 15);
temporalValues.setCalendarDate(calendarDate);
```

6. **Mapping java.time Types**:

Since Java 8, the new Java Date and Time API is available for dealing with temporal values. This API fixes many of the problems of java.util.Date and java.util.Calendar classes.

The types from the java.time package are directly mapped to corresponding SQL types.

So, there’s no need to explicitly specify @Temporal annotation:

- LocalDate is mapped to DATE.
- LocalTime and OffsetTime are mapped to TIME.
- Instant, LocalDateTime, OffsetDateTime and ZonedDateTime are mapped to TIMESTAMP.
This means that we can mark these fields only with @Basic (or @Column) annotation:

```java
@Basic
private java.time.LocalDate localDate;

@Basic
private java.time.LocalTime localTimeField;

@Basic
private java.time.OffsetTime offsetTime;

@Basic
private java.time.Instant instant;

@Basic
private java.time.LocalDateTime localDateTime;

@Basic
private java.time.OffsetDateTime offsetDateTime;

@Basic
private java.time.ZonedDateTime zonedDateTime;
```

Every temporal class in the java.time package has a static parse() method to parse the provided String value using the appropriate format.

So, here’s how we can set the values of the entity fields:

```
temporalValues.setLocalDate(LocalDate.parse("2017-11-15"));

temporalValues.setLocalTime(LocalTime.parse("15:30:18"));
temporalValues.setOffsetTime(OffsetTime.parse("08:22:12+01:00"));

temporalValues.setInstant(Instant.parse("2017-11-15T08:22:12Z"));
temporalValues.setLocalDateTime(
  LocalDateTime.parse("2017-11-15T08:22:12"));
temporalValues.setOffsetDateTime(
  OffsetDateTime.parse("2017-11-15T08:22:12+01:00"));
temporalValues.setZonedDateTime(
  ZonedDateTime.parse("2017-11-15T08:22:12+01:00[Europe/Paris]"));
```

